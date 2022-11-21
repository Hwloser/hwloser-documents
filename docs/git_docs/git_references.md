# Git References

---

如果有一个**简单的名字**可以代替SHA-1值进行git history的搜寻，那么我们就可以不用使用原始的SHA-1就可以快速的定位git log中的commit了。

在git中，这些**简单的名字**在git中被称呼为"references"或者是"refs"，`.git/refs`目录中保存了"references"到SHA-1的索引。

可以在`e.g. .git/refs/heads/master`中创建一个新的引用来保存我们最后的commit。

---

**FOR EXAMPLE**

```bash
$ echo 1a410efbd13591db07496601ebc7a059dd55cfe9 > .git/refs/heads/master
```

---

为了避免直接修改配置文件，git提供了调整的命令：

```bash
$ git update-ref refs/heads/master 1a410efbd13591db07496601ebc7a059dd55cfe9
```

---

现在，git database中存在的格式如下：

![data_model](../assets/data-model-4.png)
