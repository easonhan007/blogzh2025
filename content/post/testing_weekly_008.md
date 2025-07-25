+++
date = '2025-07-25T10:26:47+08:00'
draft = false
title = '测试周刊008: AI 在测试中有深度的应用了?'
author = "乙醇"
tags = ["测试周刊"]
weight= 1
authorLink = "https://github.com/easonhan007"
+++

## 编者思考

本期内容再次印证了软件测试行业正在经历的深刻变革。从传统的手工测试向智能化测试转变，从技术层面的优化到文化层面的重塑，我们看到测试工程师们不仅要掌握新兴的 AI 工具，更需要具备战略思维和质量意识的深度转变。

特别值得注意的是，本期多篇文章都在强调"文化"而非"技术"才是测试面临的真正挑战。这提醒我们，在关注技术进步的同时，不应忽视团队协作、质量理念和战略思维的重要性。同时，AI 驱动的测试方法论正在从概念走向实践，为我们展现了测试自动化的新可能性。

---

## 新闻资讯

### 产品开发中的 3 个关键软件测试挑战

**文章来源：** https://testuff.com/3-critical-software-testing-challenges-in-product-development

作者于其丰富的行业经验，深入分析了现代软件测试面临的三个核心挑战。这些挑战的共同特点是它们都源于组织文化层面，而非技术实现层面。作者指出，许多测试团队在技术能力上已经相当成熟，但在跨部门协作、质量意识培养和测试价值传达方面仍存在明显不足。

文章特别强调了测试人员在组织中的角色定位问题：是被动的"质量把关者"还是主动的"质量推动者"？这种角色认知的差异直接影响了测试工作的效果和团队的长远发展。作者通过具体案例说明了如何通过改变沟通方式、建立质量度量体系和培养全员质量意识来解决这些文化层面的挑战。

### QA 领域 7 年的反思

**文章来源：** https://medium.com/@putraadityapradana/two-faces-of-qa-a-reflection-after-7-years-in-the-field-5e41cf7132d9

这是一篇非常有价值的职业反思文章。作者通过 7 年的 QA 工作经验，总结出了两种截然不同的 QA 思维模式：反应式思维和战略式思维。反应式思维的特点是被动响应问题、关注局部细节、缺乏前瞻性规划；而战略式思维则强调主动预防、全局视角和长远规划。

文章详细分析了这两种思维模式在实际工作中的具体表现，以及它们对个人职业发展和团队效能的不同影响。作者特别指出，很多 QA 专业人员在技术技能上已经很熟练，但在战略思维方面还有很大提升空间。文章提供了具体的转变建议，包括如何参与产品规划、如何建立质量度量体系、如何与开发团队建立更紧密的协作关系等。

### 测试规划的定位

**文章来源：** https://medium.com/@contextdependence/orienting-test-planning-c72a84a98b46

这篇文章深入探讨了测试规划的核心要素：风险评估、测试策略制定和测试范围界定。作者认为，有效的测试规划不应该是一个孤立的技术活动，而应该与产品战略、业务目标和技术架构紧密结合。

文章特别强调了风险驱动的测试规划方法。作者详细介绍了如何识别和评估不同类型的风险（技术风险、业务风险、用户体验风险等），以及如何根据风险级别来分配测试资源和制定测试策略。同时，文章还讨论了敏捷开发环境下测试规划的特殊挑战，提出了迭代式规划和持续调整的方法论。

作者通过多个实际案例展示了良好的测试规划如何帮助团队提前发现潜在问题、优化资源配置、提高测试效率。这对于希望提升测试规划能力的专业人员来说具有很强的实践指导意义。

### Google 规模化质量：如何在数十亿行代码中控制 Bug

**文章来源：** https://bhagwatimalav.substack.com/p/quality-at-scale-how-google-keeps

这篇文章为我们揭示了 Google 这样的科技巨头是如何在极大规模的代码库中维持代码质量的。Google 的代码库包含数十亿行代码，涉及数千个项目和数万名开发人员，这种规模下的质量管理面临着前所未有的挑战。

作者详细介绍了 Google 的测试策略演进历程，从早期的手工测试为主，到后来建立完善的自动化测试体系，再到现在的 AI 辅助测试。文章特别强调了 Google 在测试文化建设方面的投入，包括"质量是每个人的责任"的理念、广泛的测试培训项目、以及将质量指标纳入绩效考核体系等。

文章还深入分析了 Google 的技术实践，包括单元测试的广泛应用（测试覆盖率要求）、集成测试的自动化、大规模并行测试的基础设施建设、以及持续集成/持续部署(CI/CD)流程的优化。这些实践对于其他企业具有很强的借鉴意义，尤其是对于正在进行数字化转型的传统企业。

### Sub-Zero Shot：彻底改变软件测试的早期上下文积累阶段

**文章来源：** https://jarbon.medium.com/sub-zero-shot-the-early-peek-thats-revolutionizing-software-testing-0f99fcfdb1d3

这是 AI 驱动测试系列文章的重要组成部分。Jason Arbon 在这篇文章中详细解析了 AI 4-Shot 测试流程中的 Sub-Zero Shot 阶段，这是整个 AI 测试流程的起始环节，也是最具创新性的部分。

Sub-Zero Shot 阶段的核心思想是让 AI 在没有任何先验知识的情况下，通过分析应用程序的界面和交互模式来自动生成测试策略。这种方法突破了传统测试需要大量人工设计测试用例的限制，让 AI 能够更早地参与到测试规划过程中。

作者详细介绍了 Sub-Zero Shot 的技术实现原理，包括计算机视觉技术在界面分析中的应用、自然语言处理在用户交互理解中的作用、以及机器学习算法在测试模式识别中的运用。文章还通过具体的案例展示了这种方法在实际应用中的效果，包括测试覆盖率的提升、测试执行时间的缩短、以及发现缺陷的质量改善。

---

## 自动化测试

### 测试框架中需要编写多态、继承和构造函数吗？

**文章来源：** https://www.reddit.com/r/QualityAssurance/comments/1laoamp/do_we_need_to_write_polymorphism_inharitance_and/

这是一个在测试开发社区中长期存在争议的话题。文章汇集了众多测试专家和开发人员的观点，深入探讨了面向对象编程(OOP)原则在测试代码中的适用性问题。

支持在测试代码中应用 OOP 原则的观点认为，多态、继承和构造函数能够提高测试代码的可维护性和复用性。特别是在大型项目中，通过继承可以建立测试基类，通过多态可以处理不同类型的测试对象，通过构造函数可以统一初始化测试环境。这些特性有助于减少代码重复，提高开发效率。

反对的观点则认为，测试代码应该保持简单直观，过度使用 OOP 特性可能会增加测试代码的复杂性，降低可读性。特别是对于测试用例，其逻辑应该尽可能线性和明确，避免过多的抽象层次。过度设计的测试框架可能会成为维护负担，甚至影响测试的稳定性。

文章总结了一个平衡的观点：在测试框架层面适度应用 OOP 原则是有益的，但在具体测试用例层面应该保持简洁。同时，团队的技术水平和项目规模也是需要考虑的重要因素。

### 技术负责人，你们是如何打破无休止手工回归测试循环的？

**文章来源：** https://www.reddit.com/r/QualityAssurance/comments/1l76mr1/tech_leads_how_did_you_break_the_cycle_of_endless

这个 Reddit 帖子反映了许多技术团队面临的共同困境：如何从繁重的手工回归测试中解脱出来。讨论中涌现出了许多实用的经验和建议，为面临类似挑战的团队提供了宝贵的参考。

讨论的核心焦点是如何在有限的资源和时间约束下，逐步建立自动化测试文化。参与讨论的技术负责人分享了他们的实际经验，包括如何获得管理层支持、如何说服开发团队投入时间进行自动化建设、以及如何处理自动化过程中遇到的技术和组织挑战。

几个关键的建议包括：从最核心的业务流程开始自动化，而不是试图一次性覆盖所有功能；建立清晰的 ROI(投资回报率)计算模型，用数据说服利益相关者；设立专门的自动化团队或指定自动化负责人；制定分阶段的自动化 roadmap；以及建立自动化测试的维护和监控机制。

讨论还涉及了不同技术栈下的自动化选择、工具评估标准、团队技能培养等实际操作层面的问题。这些来自一线实践的经验对于正在进行自动化转型的团队具有很高的参考价值。

### 使用 AI 的测试标签建议

**文章来源：** https://glebbahmutov.com/blog/test-tag-suggestions-using-ai

这篇文章展示了一个创新的 AI 应用场景：基于 pull request 描述自动推荐相关测试用例。这种方法能够显著提高测试效率，特别是在大型项目中，手工选择相关测试用例往往是一个耗时且容易出错的过程。

文章详细介绍了这个 AI 系统的工作原理：首先，系统分析 pull request 的描述文字和代码变更；然后，利用自然语言处理技术理解变更的业务含义；接着，通过机器学习算法将变更内容与测试用例库进行匹配；最后，生成测试建议并按相关性排序。

作者通过实际的代码示例和演示视频展示了这个系统的使用效果。系统不仅能够推荐直接相关的测试用例，还能够识别潜在的关联影响，推荐相关的回归测试。这种智能化的测试选择机制能够在保持测试覆盖率的同时，显著减少测试执行时间。

文章还讨论了系统的局限性和改进方向，包括如何处理复杂的业务逻辑关联、如何提高推荐准确率、以及如何与现有的 CI/CD 流程集成等。

### 编写能够讲述故事的测试用例

**文章来源：** https://blogs.rahulrpandya.in/write-tests-that-tells-the-story-33f09d743491

这篇文章从一个独特的角度探讨了测试代码的编写艺术。作者认为，好的测试不仅仅是验证功能的正确性，更应该像一个清晰的故事一样，讲述软件的预期行为和业务逻辑。

文章强调了测试命名的重要性。一个好的测试名称应该能够清楚地表达测试的目的、前置条件和预期结果。作者提供了具体的命名规则和模式，包括使用业务术语而非技术术语、采用 Given-When-Then 结构、以及避免过于抽象或模糊的描述。

在测试结构方面，文章建议采用 AAA(Arrange-Act-Assert)模式，并详细解释了每个阶段应该包含的内容。Arrange 阶段应该清晰地设置测试环境和数据；Act 阶段应该专注于执行被测试的行为；Assert 阶段应该验证所有相关的结果和副作用。

文章还讨论了测试逻辑的组织方式，强调应该避免过于复杂的测试逻辑，每个测试应该专注于验证一个明确的行为。同时，作者建议在测试中添加适当的注释，特别是对于复杂的业务规则或特殊情况的处理。

通过多个实际代码示例，文章展示了如何将抽象的原则转化为具体的编码实践，为编写高质量测试代码提供了实用的指导。

---

## 工具与技术

### webcurl: 一个非常值得学习的 postman 替代品

**项目地址**: https://github.com/o8oo8o/WebCurl

这个项目给出了一个极简版本的 postman 实现。

工具的实用性一般般，毕竟没有断言功能，只能做 api 的调试使用。

但是这个工具的源码却是异常的简单。

前端 1 个 index.html 文件，后端只有 1 个 main.go 文件。

没有任何的依赖，简单编译一下就可以运行。

非常适合普通测试人员或者测试开发工程师学习。

另外因为前后端都是单文件，遇到不懂的地方可以直接拷贝到 ai 工具中，让 ai 进行细节的讲解。

### Playwright 高级模式：并行测试和资源管理

**文章来源：** https://medium.com/@peyman.iravani/advanced-playwright-patterns-parallel-testing-and-resource-management-3e4e71e09801

这是一份深入的 Playwright 使用指南，专门针对企业级应用中的并行测试需求。随着测试套件规模的增长，如何有效地并行执行测试成为了提高 CI/CD 效率的关键因素。

文章详细介绍了 Playwright 的并行测试机制，包括测试级并行、浏览器级并行和机器级并行的不同策略。作者通过具体的配置示例展示了如何设置并行度、如何处理共享资源的竞争问题、以及如何在并行环境下维护测试的稳定性。

资源管理是并行测试的另一个重要话题。文章深入讨论了如何管理测试数据、如何处理数据库连接、如何避免端口冲突等实际问题。特别是在 Docker 环境下运行并行测试时，容器资源的分配和管理需要特殊考虑。

文章还涵盖了测试结果的聚合和报告生成，包括如何在并行执行后合并测试报告、如何处理失败测试的重试机制、以及如何监控并行测试的性能指标。作者提供了完整的配置文件示例和最佳实践建议，帮助读者在实际项目中实施高效的并行测试策略。

### PactumJS 入门：项目结构和你的第一个测试用例

**文章来源：** https://noraweisser.com/2025/06/07/getting-started-with-pactumjs-project-structure-and-your-first-test-case

PactumJS 作为一个新兴的 API 测试框架，以其简洁的语法和强大的功能吸引了越来越多的关注。这篇入门文章为初学者提供了完整的起步指南，从项目搭建到编写第一个测试用例。

文章首先介绍了 PactumJS 的核心特性，包括对 REST API 和 GraphQL 的支持、内置的断言库、数据驱动测试支持、以及与主流测试运行器的集成能力。相比于其他 API 测试工具，PactumJS 在易用性和功能完整性之间找到了很好的平衡点。

在项目结构方面，作者详细说明了如何组织测试文件、配置文件和测试数据。文章提供了一个标准的项目模板，包括测试环境配置、公共工具函数、测试数据管理等。这种结构化的方法有助于维护大型 API 测试套件。

文章的核心部分是实际的测试用例编写。作者通过一个完整的示例展示了如何使用 PactumJS 进行 API 测试，包括请求构建、响应验证、错误处理等。特别值得注意的是，PactumJS 提供了丰富的断言方法和数据验证功能，能够满足复杂 API 测试的需求。

最后，文章讨论了与 CI/CD 流程的集成，以及如何生成测试报告和监控 API 测试结果。

### 从使用 Cursor 和 Windsurf 中学到的质量工程经验

**文章来源：** https://www.ministryoftesting.com/articles/lessons-in-quality-engineering-from-working-with-cursor-and-windsurf

AI 驱动的开发工具正在改变软件开发的方式，但它们对代码质量的影响如何？z 作者过实际使用 Cursor 和 Windsurf 两款 智能编程 IDE 的经验，为我们提供了第一手的观察和分析。

文章详细比较了这两款工具的特性和使用体验。Cursor 以其强大的代码补全和重构能力著称，而 Windsurf 则在代码审查和质量分析方面表现出色。作者通过实际的编码项目测试了这些工具在不同场景下的表现，包括新功能开发、Bug 修复、代码重构等。

从质量工程的角度，文章重点分析了 AI 工具对代码质量的双重影响。积极方面包括：提高编码效率、减少低级错误、提供最佳实践建议、以及帮助开发者学习新技术。消极方面则包括：可能产生难以理解的代码、过度依赖可能降低开发者的思考能力、以及 AI 生成代码的质量不稳定性。

作者特别强调了人工 review 在 AI 辅助开发中的重要性。即使有了强大的 AI 工具，人工的代码审查和质量把控仍然不可替代。文章提供了在使用 AI 工具时如何保持代码质量的具体建议，包括设置适当的提示词、建立代码 review 流程、以及持续监控代码质量指标。

### 使用 Testcontainers 进行数据库 Python 单元测试

**文章来源：** https://medium.com/@gavinklfong/python-unit-test-on-database-with-testcontainers-315975f04df4

数据库测试一直是单元测试中的一个难点，传统的 mock 方式往往无法完全模拟真实数据库的行为，而使用真实数据库又面临环境配置和数据隔离的挑战。Testcontainers 的出现为这个问题提供了优雅的解决方案。

文章详细介绍了 Testcontainers 的工作原理：通过 Docker 容器技术，为每个测试提供独立的数据库实例。这种方法既保证了测试环境的一致性，又避免了测试之间的数据污染。同时，容器的快速启动和销毁特性使得测试执行效率得到了很好的保障。

作者通过一个完整的 Python 项目示例展示了如何集成 Testcontainers。示例涵盖了项目配置、数据库初始化、测试数据准备、测试执行和清理等完整流程。特别值得注意的是，文章详细说明了如何处理数据库 schema 的创建和迁移、如何管理测试数据的生命周期、以及如何在 CI/CD 环境中运行这些测试。

文章还讨论了性能优化的策略，包括容器复用、并行测试支持、以及资源限制配置等。这些优化措施对于大型项目的测试效率至关重要。

---

## 视频资源

### 软件测试中的代理 AI：认识 TestZeus Hercules

**视频来源：** https://www.youtube.com/watch?v=uc6z8Uaqfw8

这个 12 分钟的视频介绍了 TestZeus Hercules 这一开源的 AI 测试工具。

作为 AI 驱动测试的最新发展，TestZeus Hercules 代表了测试自动化的新方向：从简单的脚本执行向智能决策转变。

视频详细演示了 TestZeus Hercules 的核心功能，包括自动测试用例生成、智能测试执行、结果分析和报告生成等。特别令人印象深刻的是其自然语言交互能力，测试人员可以用日常语言描述测试需求，系统会自动生成相应的测试策略和执行计划。

视频中分享了这个项目的技术架构和实现原理，包括如何集成多种 AI 技术（计算机视觉、自然语言处理、机器学习等）来实现 AI 自动化测试。

视频还展示了实际的使用场景，从 Web 应用测试到移动应用测试，TestZeus Hercules 都展现出了良好的通用性。

## 总结

期周刊突出了软件测试领域的几个重要趋势：

### 文化转变的重要性

多篇文章强调，测试面临的真正挑战不在技术层面，而在于团队文化和思维模式的转变。从被动式测试向战略式测试思维的转变，以及获得团队对质量工作的认同，这些"软实力"比掌握新工具更为关键。

### AI 在测试中的深度应用

从 AI 辅助的测试标签建议到 TestZeus Hercules 等 AI 自动化测试框架。

我们看到 AI 正在从辅助工具向自主决策方向发展。这种变化不仅提高了测试效率，更重要的是改变了测试工程师的工作方式。

### 大规模质量管理的实践分享

Google 等大型企业的质量管理经验为我们提供了宝贵的参考。这些实践告诉我们，规模化的质量管理需要系统性的思考和长期的文化建设。

### 工具生态的持续完善

从 Playwright 的并行测试到新的 API 验证工具，测试工具链正在变得更加成熟和专业化。这为测试工程师提供了更多选择，同时也要求我们具备更广泛的技术视野。
