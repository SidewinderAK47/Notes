## 转账过程中存在的事务问题：

![事务控制](Untitled.assets/事务控制.png)这就

也就是说，整个过程中  需要获取4次 数据库连接Connection。
每个Connection都有自己独立的事务。
连接1 成功：提交
连接2 成功：提交
连接3 成功：提交
出现异常：
连接4 无法继续运行下去了。

因此：4个操作应该由一个数据库连接来操作。要成功则，4个操作一起成功； 要失败则，4个操作一起失败。

需要使用ThreadLocal对象，把Connection对象和当前线程绑定，使得一个线程中只有一个能控制事务的对象。





通过一系列的努力，事务又开始正常了。转账开始正确执行了。

但是这里面

- 配置变得很麻烦。

- 依赖变得乱七八糟。

### 最初的AccountService实现类存在的问题：
```java
public class AccountServiceImpl_OLD implements IAccountService {

    private IAccountDao accountDao;
    private TransactionManager txManager; //使用Spring依赖注入进来

    public void setTxManager(TransactionManager txManager) {
        this.txManager = txManager;
    }

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public List<Account> findAllAcount() {
            return accounts=  accountDao.findAllAcount();
    }
    @Override
    public Account findAccountById(Integer accountId) {

          return accountDao.findAccountById(accountId);;

    }
    @Override
    public void saveAccount(Account account) {

           accountDao.saveAccount(account);

    }
    @Override
    public void updateAccount(Account account) {
        accountDao.updateAccount(account);

    }
    @Override
    public void deleteAccount(Integer accountId) {
        accountDao.deleteAccount(accountId);
    }

    @Override
    public void transfer(String sourceName, String targetName, Float money) {
            accountDao.updateAccount(target);
    }

}
```
当涉及到多次操作的时候，不能实现功能，由于每次获取一个连接，导致没法实现事务控制。
能实现功能，除了重复代码，还有什么问题要我们解决呢？

认真思考，依赖有类之间的依赖，也就是调用依赖，也有方法之家你的依赖。
比如方法beginTransaction方法，变成beginTransaction1后，
原始的serviceImpl没有任何问题，但是后来改建的serviceImpl方法要改的地方就很多，方法之间有很多依赖关系。我们在实际开发过程过可能会写几十个service，因此我们要尽量保持方法之间的独立动态
