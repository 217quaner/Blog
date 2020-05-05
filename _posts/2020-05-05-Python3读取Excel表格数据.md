# Python3读取Excel表格数据
## 步骤
1. 导入xlrd包
2. 打开excel文件
3. 获取全部工作表（sheet）
4. 选取对应的工作表
5. 选取所需数据

## 示例
```python
# 导入xlrd包
import xlrd


def read_excel(self):
    # 打开excel文件
    workbook = xlrd.open_workbook(r'./test.xlsx')

    # 获取全部工作表（sheet）以及选取对应的工作表
    sheet_name = workbook.sheet_names()[0]
    host_sheet = workbook.sheet_by_name(sheet_name)

    rows = host_sheet.nrows
    return host_sheet, rows

# 选取数据
def read_data(self, host_sheet, rows):
    host_data = []
    for row in range(rows):
        host_data += [host_sheet.row_values(row)]
    return host_data[1:]

if __name__ == '__main__':
    host_sheet, rows = self.read_excel()
    host_datas = self.read_data(host_sheet, rows)
```
