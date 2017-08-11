# MoreStyleForTableViewCell
优点：代码清晰。
缺点：文件偏多。
// Controller中只需要一些简单的设置，代理在各个Helper中实现，更加便于查看、变更。
// 随意写的简单示例，示例代码还可以优化！




================================================================================================
- (void)viewDidLoad {
    [super viewDidLoad];
    self.moreStyleCellManager = [[MoreStyleCellManager alloc] initWithTableView:self.tableView viewController:self];
    StyleOneHelper *helper = [[StyleOneHelper alloc] init];
    helper.titleArr = [@[@"猫",@"狗",@"猪",@"鸡",@"鸭"] mutableCopy];
    
    StyleSecendHelper *helper2 = [[StyleSecendHelper alloc] init] ;
    helper2.titleArr = [@[@"狼",@"虎",@"豹"] mutableCopy];
    [self.moreStyleCellManager addMoreStyleDelegate:helper];
    [self.moreStyleCellManager addMoreStyleDelegate:helper2];
                            
    [self.view addSubview:self.tableView];
    self.tableView.frame = self.view.bounds;
}

- (UITableView *)tableView{
    if (!_tableView) {
        _tableView = [[UITableView alloc] initWithFrame:CGRectZero style:UITableViewStylePlain];
        _tableView.tableFooterView = [[UIView alloc] init];
    }
    return _tableView;
}

MoreStyleCellManager
================================================================================================
- (instancetype)initWithTableView:(UITableView *)tableView viewController:(UIViewController *)viewcontroller
{
    self = [super init];
    if (self) {
        self.tableView = tableView;
        self.viewController = viewcontroller;
        self.tableView.delegate = self;
        self.tableView.dataSource = self;
    }
    return self;
}

- (MoreStyleCellManager *)addMoreStyleDelegate:(StyleBaseHelper *)delegate {
    [self.moreStyleCellDelegateArr addObject:delegate];
    delegate.tableView = self.tableView;
    delegate.viewController = self.viewController;
    return self;
}

- (NSMutableArray *)moreStyleCellDelegateArr{
    if (!_moreStyleCellDelegateArr) {
        _moreStyleCellDelegateArr = [[NSMutableArray alloc] init];
    }
    return _moreStyleCellDelegateArr;
}

#pragma mark - ___________________  tableViewDelegate
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView{
    return self.moreStyleCellDelegateArr.count;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    return [self.moreStyleCellDelegateArr[section] numberOfRowsInSection:section];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    UITableViewCell * cell = [self.moreStyleCellDelegateArr[indexPath.section] cellForRowAtIndexPath:indexPath];
    return cell;
}

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
    [self.moreStyleCellDelegateArr[indexPath.section] didSelectRowAtIndexPath:indexPath];
}
@end

Helper
================================================================================================
- (NSInteger)numberOfRowsInSection:(NSInteger)section{
    return self.titleArr.count;
}

- (UITableViewCell *)cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    UITableViewCell *cell = [self.tableView dequeueReusableCellWithIdentifier:self.identifier];
    if (cell == nil) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:self.identifier];
    }
    cell.textLabel.text = self.titleArr[indexPath.row];
    return cell;
}

- (void)didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    NSLog(@"%@：   --->  叫了一声！",self.titleArr[indexPath.row]);
}

================================================================================================

