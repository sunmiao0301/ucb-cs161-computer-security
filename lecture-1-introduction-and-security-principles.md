##### Lecture 1: Introduction

#####  course structure

- intro to security
- memory safety
- cryptography
- network security 
- web security
- miscellaneous topics 

除了False positive，还有False Negatives。

##### Reading 01

- 安全就是经济
- 最小权限原则
- 使用即使失败也安全的默认设计，比如防火墙，防火墙崩溃时不会开放所有端口，而是关闭所有端口。
- 责任分离，为什么电影院要一个售票，一个拿走票根，这样方便对账，降低贪污。
- 深度防御（额外的保险）
- 考虑安全措施的心理承受能力，如果一个公司要求所有人密码都是一个17位的随机生成密码，并且每个月修改一次，那么最后只会导致所有员工把密码记在便利贴上，贴在显示器上。——适得其反
- 考虑人为因素，也就是采用人性化的安全设计。
- 完整的安全 —— 安全也有木桶效应
- 清楚你的**threat model**
- 如果你无法避免，那么至少监测到 —— 昂贵的器材可以保证不被篡改，便宜的器材可以看到是否被篡改，那么销量更好的会是后者。
- 不要依赖借助保密换来的安全 —— 反例是RSA，所有人都知道RSA的原理，但无法快速攻破。
- 从一开始就设计安全 —— 想要对已经完成的系统加上安全，你会被**向后兼容**折磨
- 考虑在你的threat model下，最严重的攻击情况
- **Kerkhoff’s principle:** Cryptosystems should remain secure even when the attacker knows all internal details of the system.
- 主动学习攻击 —— 自己尝试打破自己的系统，这样在真正的敌人来临前有更多准备时间。