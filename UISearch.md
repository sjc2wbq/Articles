---
title: UISearch
date: 2016-02-10 09:35:22
tags:
---
# iOS8之前

```objc
 wait to add
```

# iOS8之后
```objc
@interface ViewController ()<UISearchResultsUpdating,UISearchBarDelegate>
@property(nonatomic) UISearchController *searchC;

- (UISearchController *)searchC{
    if (!_searchC) {
        _searchC = [[UISearchController alloc]initWithSearchResultsController:[TableViewController new]];
        _searchC.searchBar.scopeButtonTitles = @[@"Device",@"Soft",@"Other"];
        //设置搜索结果发生变化时，代理由谁负责
        _searchC.searchResultsUpdater = self;
#warning 应对scope变化时的无法监听的问题
        _searchC.searchBar.delegate = self;
    }
    return _searchC;
}

#warning 应对scope变化时的无法监听的问题
#pragma mark - UISearchBar Delegate
-(void)searchBar:(UISearchBar *)searchBar selectedScopeButtonIndexDidChange:(NSInteger)selectedScope{
    [self updateSearchResultsForSearchController:self.searchC];
}

#warning 实际上scope选项变化时并没有触发
-(void)updateSearchResultsForSearchController:(UISearchController *)searchController{
    NSString *text = searchController.searchBar.text;
    NSInteger selectedIndex = searchController.searchBar.selectedScopeButtonIndex;
    NSMutableArray *tmpArr = [NSMutableArray new];
    for (Product *p in self.dataList) {
        if ([p.name containsString:text] && p.type == selectedIndex) {
            [tmpArr addObject:p];
        }
    }
    TableViewController *vc = (TableViewController *)searchController.searchResultsController;
    vc.resultArr = tmpArr;

}
```
