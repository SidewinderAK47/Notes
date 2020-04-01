

LSTM代码案例（基于PyTorch实现）：

## 1 输入一段话，输出这句话中每个单词词性

```python
'''
本程序实现了对单词词性的判断，输入一句话，输出该句话中每个单词的词性。
'''
 
import torch
import torch.nn.functional as F
from torch import nn, optim
 
training_data = [("The dog ate the apple".split(), ["DET", "NN", "V", "DET", "NN"]),
                 ("Everybody read that book".split(), ["NN", "V", "DET", "NN"])]
# DET,NN,V 一共三个 
word_to_idx = {} # map<word,idx>
tag_to_idx = {} # map<tag,idx>
for context, tag in training_data: # 词汇列表，词性
    for word in context: # 遍历单词
        if word not in word_to_idx:
            word_to_idx[word] = len(word_to_idx) # 给每个单词，赋予一个唯一id
    for label in tag:  # 遍历词性标签
        if label not in tag_to_idx: # 同理给每个词性赋予一个唯一标签。
            tag_to_idx[label] = len(tag_to_idx)
idx_to_tag = {tag_to_idx[tag]: tag for tag in tag_to_idx}# 因为两者其实都是唯一的，所以这边采用 map<idx,tag>方便反向查找；
 
 
class LSTMTagger(nn.Module):
    def __init__(self, n_word, n_dim, n_hidden, n_tag):
        super(LSTMTagger, self).__init__()
        self.word_embedding = nn.Embedding(n_word, n_dim)
        self.lstm = nn.LSTM(input_size=n_dim, hidden_size=n_hidden, batch_first=True)  # nn.lstm()接受的数据输入是(序列长度，batch，输入维数)，
                                                                # 这和我们cnn输入的方式不太一致，所以使用batch_first，  （相当于CNN的黑盒，不想关）
                                                                # 我们可以将输入变成(batch，序列长度，输入维数)
        self.linear1 = nn.Linear(n_hidden, n_tag)
 
    def forward(self, x):            # x是word_list,即单词的索引列表，size为([len(x)])
        x = self.word_embedding(x)   # embedding之后，x的size为([len(x),n_dim])
        #print('-------------')
        # print("x.size() ",x.size()) torch.Size([5, 100])
        # 增加一个维度，最外层
        x = x.unsqueeze(0)           # unsqueeze之后，x的size为([1,len(x),n_dim]),1在下一行程序的lstm中被当做是batchsize，len(x)被当做序列长度  
        # print("x.size() ",x.size())  torch.Size([1, 5, 100])
        # lstm输入有前面定义为: (batch, time_step, input_size)
        # lstm输出为r_out,(h_n, h_a) r_out格式为[batch_size,hidden_size,dim]
        x, _ = self.lstm(x)          # lstm的隐藏层输出，x的size为([1,len(x),n_hidden])
        # 因为batch_size = 1直接 压缩
        # 在第0个维度，减少一个维度
        x = x.squeeze(0)             # squeeze之后，x的size为([len(x),n_hidden])，在下一行的linear层中，len(x)被当做是batchsize 因为是batch_size =1 因此，压缩一下就行了
        x = self.linear1(x)          # linear层之后，x的size为([len(x),n_tag])
        #print('x')
        #print(x)
        y = F.log_softmax(x, dim=1)  # 对第1维进行softmax计算，y的size为([len(x),n_tag])
        #print('y')
        #print(y)
        return y
 
 
model = LSTMTagger(n_word=len(word_to_idx), n_dim=100, n_hidden=128, n_tag=len(tag_to_idx))  # 设置模型，n_dim = 100 , n_hidden = 128 n_tag = 
if torch.cuda.is_available():
    model = model.cuda()
criterion = nn.CrossEntropyLoss() # 评判标准
optimizer = optim.SGD(model.parameters(), lr=1e-2)# 优化器，学习率，学习的参数
 
 
for epoch in range(200):
    running_loss = 0
    for data in training_data:
        sentence, tags = data
        word_list = [word_to_idx[word] for word in sentence]     # word_list是word索引列表  将句子中的每个单词 替换成 唯一的id
        word_list = torch.LongTensor(word_list)              # 转换成张量
        tag_list = [tag_to_idx[tag] for tag in tags]         # tag_list是tag索引列表   将词性标签 转换成 唯一的id 
        tag_list = torch.LongTensor(tag_list)               # 转换成张量
        if torch.cuda.is_available():
            word_list = word_list.cuda()
            tag_list = tag_list.cuda()
        # forward
        out = model(word_list)
        loss = criterion(out, tag_list)
        running_loss += loss.data.numpy()
        # backward
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    print('Epoch: {:<3d} | Loss: {:6.4f}'.format(epoch, running_loss / len(data)))
 
# 模型测试
test_sentence = "Everybody ate the apple"
print('\n The test sentence is:\n', test_sentence)
test_sentence = test_sentence.split()
test_list = [word_to_idx[word] for word in test_sentence]
test_list = torch.LongTensor(test_list)

if torch.cuda.is_available():
    test_list = test_list.cuda()

out = model(test_list) # 
print('out.size',out.size()) # out.size torch.Size([4, 3]) 
_, predict_idx = torch.max(out, 1)  # 1表示找行的最大值。 predict_idx是词性索引，是一个size为([len(test_sentence)]的张量
#torch.max(input) → Tensor  返回输入tensor中所有元素的最大值
#torch.max(input, dim, keepdim=False, out=None) -> (Tensor, LongTensor) （最大值，索引）
#按维度dim 返回最大值
#torch.max(a,0) 返回每一列中最大值的那个元素，且返回索引（返回最大元素在这一列的行索引）

# 选择每个单词预测索引[]中的最大值作为预测结果
predict_tag = [ idx_to_tag[idx] for idx in list(predict_idx.numpy()) ] 

print('The predict tags are:', predict_tag)
```



## 2 RNN进行手写数字的识别：

主要思路：

从一个torchvision数据集里面取的手写数字0-9 10个数字类别：  分类问题；

训练集：6万张图片；图片格式设置为28*28  batch_size 设置为64

损失函数为：交叉熵损失函数。

测试集：2000张图片；

网络结构：

```out
RNN(
  (rnn): LSTM(28, 64, batch_first=True)
  (out): Linear(in_features=64, out_features=10, bias=True)
)
```



```python
import torch
from torch import nn
from torch.autograd import Variable
import torchvision.datasets as dsets
import torchvision.transforms as transforms
import matplotlib.pyplot as plt
%matplotlib inline

torch.manual_seed(1)    # reproducible

# Hyper Parameters
EPOCH = 1               # train the training data n times, to save time, we just train 1 epoch
BATCH_SIZE = 64
TIME_STEP = 28          # rnn time step / image height
INPUT_SIZE = 28         # rnn input size / image width
LR = 0.01               # learning rate
DOWNLOAD_MNIST = True   # set to True if haven't download the data

# Mnist digital dataset
train_data = dsets.MNIST(
    root='./mnist/',
    train=True,                         # this is training data
    transform=transforms.ToTensor(),    # Converts a PIL.Image or numpy.ndarray to
                                        # torch.FloatTensor of shape (C x H x W) and normalize in the range [0.0, 1.0]
    download=DOWNLOAD_MNIST,            # download it if you don't have it
)

# plot one example
print(train_data.train_data.size())     # torch.Size([60000, 28, 28])  训练时6万张
print(train_data.train_labels.size())   # torch.Size([60000]) train_labels是[] 每一个

plt.imshow(train_data.train_data[0].numpy(), cmap='gray') # 第0 是一张图片 28*28
plt.title('%i' % train_data.train_labels[0])  # 每一维度是 图片的正确数字标签
plt.show()

# Data Loader for easy mini-batch return in training
train_loader = torch.utils.data.DataLoader(dataset=train_data, batch_size=BATCH_SIZE, shuffle=True) # 设置了batch_size

# convert test data into Variable, pick 2000 samples to speed up testing
test_data = dsets.MNIST(root='./mnist/', train=False, transform=transforms.ToTensor())
test_x = Variable(test_data.test_data, volatile=True).type(torch.FloatTensor)[:2000]/255.   # shape (2000, 28, 28) value in range(0,1) 测试是2000张
test_y = test_data.test_labels.numpy()[:2000]    # shape torch.Size([10000])   covert to numpy array sequeeze 维度减少[],unsequeeze 维度增加 .squeeze()
print("-"*55)
print(test_data.test_labels.size())

class RNN(nn.Module):
    def __init__(self):
        super(RNN, self).__init__()

        self.rnn = nn.LSTM(         # if use nn.RNN(), it hardly learns
            input_size=INPUT_SIZE,
            hidden_size=64,         # rnn hidden unit
            num_layers=1,           # number of rnn layer
            batch_first=True,       # input & output will has batch size as 1s dimension. e.g. (batch, time_step, input_size)
        )

        self.out = nn.Linear(64, 10)    # 输入是64，输出是10维

    def forward(self, x):
        # x shape (batch, time_step, input_size)
        # r_out shape (batch, time_step, output_size) torch.Size([64, 28, 64]) 没个时间序列的out都存储在这边 隐藏层1层size定义为64，输出应该也是64，batchsize=64,time_step=28 每部output_size=64
        # h_n shape (n_layers, batch, hidden_size)   torch.Size([1, 64, 64])
        # h_c shape (n_layers, batch, hidden_size)   变量什么意思不懂啊 torch.Size([1, 64, 64]) 中间变量输出：定义隐藏层数为1层，batch_size=64,
        r_out, (h_n, h_c) = self.rnn(x, None)     # None represents zero initial hidden state
        # print(h_c.size()) 
        # choose r_out at the last time step    torch.Size([64, 28, 64])
        tmp = r_out[:, -1, :]             # torch.Size([64, 64]) 取time_step最后的output, batch_size=64,隐藏层1层size定义为64
        # print(tmp.size())          
        out = self.out(tmp)               # torch.Size([64, 10]) 输入28组数据，最后只输出最后一组结果
        #print('out.size():',out.size())
        return out

rnn = RNN()
print(rnn)



optimizer = torch.optim.Adam(rnn.parameters(), lr=LR)   # optimize all cnn parameters
loss_func = nn.CrossEntropyLoss()                       # the target label is not one-hotted

# print(train_loader[0])
# training and testing
for epoch in range(EPOCH):
    for step, (x, y) in enumerate(train_loader):        # gives batch data 批数据大小时64张图片
        # print(x.size()) # torch.Size([64, 1, 28, 28])
        # print(y) # torch.Size([64])
        b_x = Variable(x.view(-1, 28, 28))              # reshape x to (batch, time_step, input_size)  [64,28,28]
        b_y = Variable(y)                       # [68]
        #print(b_y.size())                               # batch y

        output = rnn(b_x)
        #print(output.size())
        #print(y.size())                               # rnn output
        loss = loss_func(output, b_y)                   # cross entropy loss
        optimizer.zero_grad()                           # clear gradients for this training step
        loss.backward()                                 # backpropagation, compute gradients
        optimizer.step()                                # apply gradients

        if step % 50 == 0:
            test_output = rnn(test_x)                   # (samples, time_step, input_size)
            print('test_output:\n',test_output.size())
            pred_y = torch.max(test_output, 1)[1].data.numpy().squeeze()
            accuracy = sum(pred_y == test_y) / float(test_y.size)
            accuracy
            epoch
            loss.data
           # print('Epoch: ', epoch, '| train loss: %.4f' % loss.data[0], '| test accuracy: %.2f' % accuracy)


# print 10 predictions from test data
print("+"*20)
print(test_x[:10].size()) # torch.Size([10, 28, 28])
test_output = rnn(test_x[:10].view(-1, 28, 28)) # 取test_x的前10个图片进行测试 ,加个view也是torch.Size([10, 28, 28])
# test_output : rnn输出 torch.Size([10, 10]) batch_size=10，每张图片就是10维
pred_y = torch.max(test_output, 1)[1].data.numpy().squeeze() #torch.max（，dim） dim=0返回行最大值，dim=返回列最大值；：元组（最大值，索引）
print(pred_y, 'prediction number')
print(test_y[:10], 'real number')
```

部分输出

```out
torch.Size([60000, 28, 28])
torch.Size([60000])
/usr/local/lib/python3.6/dist-packages/torchvision/datasets/mnist.py:55: UserWarning: train_data has been renamed data
  warnings.warn("train_data has been renamed data")
/usr/local/lib/python3.6/dist-packages/torchvision/datasets/mnist.py:45: UserWarning: train_labels has been renamed targets
  warnings.warn("train_labels has been renamed targets")

/usr/local/lib/python3.6/dist-packages/torchvision/datasets/mnist.py:60: UserWarning: test_data has been renamed data
  warnings.warn("test_data has been renamed data")
/usr/local/lib/python3.6/dist-packages/ipykernel_launcher.py:41: UserWarning: volatile was removed and now has no effect. Use `with torch.no_grad():` instead.
/usr/local/lib/python3.6/dist-packages/torchvision/datasets/mnist.py:50: UserWarning: test_labels has been renamed targets
  warnings.warn("test_labels has been renamed targets")
-------------------------------------------------------
torch.Size([10000])
RNN(
  (rnn): LSTM(28, 64, batch_first=True)
  (out): Linear(in_features=64, out_features=10, bias=True)
)
test_output:
 torch.Size([2000, 10])
```



```out
torch.Size([10, 28, 28])
torch.Size([10, 28, 28])
tensor([[-3.5679e+00, -2.8431e-01, -1.2612e-02, -3.1977e-01, -4.7850e-01,
         -2.9990e+00, -2.7163e+00,  7.2240e+00, -1.6450e+00,  2.2447e+00],
        [-1.5551e+00,  8.2569e-01,  7.3710e+00,  7.8814e-01, -1.7266e+00,
         -2.1462e+00,  3.3151e-02,  4.3457e-01,  2.2529e-01, -3.5428e+00],
        [-2.8322e+00,  5.1068e+00,  1.1052e+00, -1.5436e+00, -6.9034e-01,
         -1.1297e+00,  2.5248e-01, -1.4892e-01,  2.2403e+00, -2.9282e+00],
        [ 9.2911e+00, -5.7281e+00, -7.3139e-01, -6.9566e-01, -1.2305e+00,
         -9.3338e-01,  4.6703e-01, -3.7078e+00, -9.6071e-01,  1.6777e+00],
        [-8.4168e-01, -2.3530e+00, -1.1360e+00, -3.7166e+00,  7.7780e+00,
         -4.0664e-01, -3.2254e-01, -1.4607e+00, -2.5428e+00,  2.6750e+00],
        [-3.1442e+00,  7.3571e+00, -4.1672e-02, -1.1495e+00, -1.0709e+00,
         -1.5400e+00, -1.3474e+00,  9.2124e-01,  1.6098e+00, -2.7568e+00],
        [-2.2458e+00, -3.3269e-01, -1.4495e+00, -3.0634e+00,  7.2441e+00,
         -1.4061e+00, -5.2196e-01,  8.3100e-01, -2.4226e+00,  2.0681e+00],
        [-1.6500e+00, -2.1394e+00, -1.3710e+00, -7.4594e-01,  7.0973e-01,
         -5.0767e-01, -2.3568e+00, -5.2031e-03, -8.8278e-01,  5.2219e+00],
        [-9.5751e-01, -2.4880e+00, -2.4228e+00, -1.1112e+00, -1.1428e+00,
          6.0236e+00,  5.0592e+00, -5.2559e+00,  1.0997e+00, -3.4395e-01],
        [-2.2273e+00, -2.0131e+00, -4.0603e+00, -1.2842e+00,  2.6969e+00,
         -1.2078e+00, -1.5398e+00, -3.4321e-01, -4.4965e-02,  7.2916e+00]],
       grad_fn=<AddmmBackward>)
torch.max(test_output, 1)
 torch.return_types.max(
values=tensor([7.2240, 7.3710, 5.1068, 9.2911, 7.7780, 7.3571, 7.2441, 5.2219, 6.0236,
        7.2916], grad_fn=<MaxBackward0>),
indices=tensor([7, 2, 1, 0, 4, 1, 4, 9, 5, 9]))
tensor([[-3.5679e+00, -2.8431e-01, -1.2612e-02, -3.1977e-01, -4.7850e-01,
         -2.9990e+00, -2.7163e+00,  7.2240e+00, -1.6450e+00,  2.2447e+00],
        [-1.5551e+00,  8.2569e-01,  7.3710e+00,  7.8814e-01, -1.7266e+00,
         -2.1462e+00,  3.3151e-02,  4.3457e-01,  2.2529e-01, -3.5428e+00],
        [-2.8322e+00,  5.1068e+00,  1.1052e+00, -1.5436e+00, -6.9034e-01,
         -1.1297e+00,  2.5248e-01, -1.4892e-01,  2.2403e+00, -2.9282e+00],
        [ 9.2911e+00, -5.7281e+00, -7.3139e-01, -6.9566e-01, -1.2305e+00,
         -9.3338e-01,  4.6703e-01, -3.7078e+00, -9.6071e-01,  1.6777e+00],
        [-8.4168e-01, -2.3530e+00, -1.1360e+00, -3.7166e+00,  7.7780e+00,
         -4.0664e-01, -3.2254e-01, -1.4607e+00, -2.5428e+00,  2.6750e+00],
        [-3.1442e+00,  7.3571e+00, -4.1672e-02, -1.1495e+00, -1.0709e+00,
         -1.5400e+00, -1.3474e+00,  9.2124e-01,  1.6098e+00, -2.7568e+00],
        [-2.2458e+00, -3.3269e-01, -1.4495e+00, -3.0634e+00,  7.2441e+00,
         -1.4061e+00, -5.2196e-01,  8.3100e-01, -2.4226e+00,  2.0681e+00],
        [-1.6500e+00, -2.1394e+00, -1.3710e+00, -7.4594e-01,  7.0973e-01,
         -5.0767e-01, -2.3568e+00, -5.2031e-03, -8.8278e-01,  5.2219e+00],
        [-9.5751e-01, -2.4880e+00, -2.4228e+00, -1.1112e+00, -1.1428e+00,
          6.0236e+00,  5.0592e+00, -5.2559e+00,  1.0997e+00, -3.4395e-01],
        [-2.2273e+00, -2.0131e+00, -4.0603e+00, -1.2842e+00,  2.6969e+00,
         -1.2078e+00, -1.5398e+00, -3.4321e-01, -4.4965e-02,  7.2916e+00]],
       grad_fn=<AddmmBackward>)
torch.return_types.max(
values=tensor([7.2240, 7.3710, 5.1068, 9.2911, 7.7780, 7.3571, 7.2441, 5.2219, 6.0236,
        7.2916], grad_fn=<MaxBackward0>),
indices=tensor([7, 2, 1, 0, 4, 1, 4, 9, 5, 9]))
torch.max(test_output, 1)[1].data.numpy()
 [7 2 1 0 4 1 4 9 5 9]
```

