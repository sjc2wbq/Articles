---
title: 'tableViewCell、collectionViewCell '
tags: tableView
---
## 目录
- 注册重用法
- tableview自适应高度
- tableview撤销高亮
- 参数的传递
  1. 纯代码（didselectRow）
  2. storyBoard方式（prepareForSegue）

### tableViewCell、collectionViewCell重用注册法  
> 1.  **纯代码**注册时用**registerClass**如下图示例
2.  **xib界面**注册时用**registerNib**作(tableView或collectionView)的cell  
3.  **storyboard**

-  #### 注册方法

```objc
[self.myCollectionView registerClass:[MycollectionViewCell class]
         forCellWithReuseIdentifier:@"cell"];

[self.myCollectionView registerNib:[UINib nibWithNibName:@"mycollectionCell"
                            bundle:nil]
         forCellWithReuseIdentifier:cellIndentify];
  ```

<!--more-->
- #### 重用方法

```objc
MycollectionViewCell *cell;
cell = [collectionView dequeueReusableCellWithReuseIdentifier:cellIndentify
                                                  forIndexPath:indexPath];
```
### 让tableViewCell横线没有空隙
```objc
cell.separatorInset = UIEdgeInsetsZero;
cell.layoutMargins = UIEdgeInsetsZero;
cell.preservesSuperviewLayoutMargins = NO;
```
### tableview自适应高度
```objc
-(CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath{

    return UITableViewAutomaticDimension;
}
```
