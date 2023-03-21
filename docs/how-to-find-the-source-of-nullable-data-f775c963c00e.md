# 如何找到可空数据的来源，在 IntelliJ IDEA 中跟踪和分析数据流

> 原文：<https://medium.com/javarevisited/how-to-find-the-source-of-nullable-data-f775c963c00e?source=collection_archive---------0----------------------->

![](img/5fa8823ab024546516dd7751ff0fa9c7.png)

照片由 [magnezis magnestic](https://unsplash.com/@agneska?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/footprints?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你还记得，[在我的一篇文章](/p/there-is-a-place-for-a-null-check-2baca2eb98ae?source=email-ef64c6fabfb2--writer.postDistributed&sk=c0d935e94529fe737c630aad5574f144)中，我谈到了数据生产者和数据消费者之间的契约中的空支票。在那里，我提出了一个观点，即最好在检索到数据后立即检查数据，以便决定所提供的数据是否适合处理。