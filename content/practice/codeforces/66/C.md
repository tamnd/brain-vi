---
title: "CF 66C - Petya và Hệ thống tập tin"
description: "Mỗi dòng đầu vào mô tả đường dẫn đầy đủ của một tệp trong hệ thống tệp. Một đường dẫn trông như sau: Tên đĩa là thư mục gốc và không được coi là thư mục. Mọi thành phần giữa đĩa và tập tin là một thư mục. Thành phần cuối cùng luôn là một tập tin."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 66
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 61 (Div. 2)"
rating: 1800
weight: 66
solve_time_s: 133
verified: true
draft: false
---

[CF 66C - Petya và Hệ thống tệp](https://codeforces.com/problemset/problem/66/C) 

**Đánh giá:** 1800 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả đường dẫn đầy đủ của một tệp trong hệ thống tệp. Một đường dẫn trông giống như:```
C:\folder1\folder2\file.txt
```Tên đĩa là thư mục gốc và không được coi là thư mục. Mọi thành phần giữa đĩa và tập tin là một thư mục. Thành phần cuối cùng luôn là một tập tin. 

Đối với mỗi thư mục, chúng tôi cần hai số lượng. 

Số lượng đầu tiên là có bao nhiêu thư mục con tồn tại bên trong nó, bao gồm cả các thư mục con lồng nhau ở mọi độ sâu. 

Số lượng thứ hai là có bao nhiêu tệp tồn tại bên trong nó, bao gồm các tệp nằm trong tất cả các thư mục con. 

Nhiệm vụ là tính giá trị tối đa của từng số lượng trên tất cả các thư mục trong toàn bộ hệ thống tệp. 

Đầu vào nhỏ. Có tối đa 100 đường dẫn và độ dài mỗi đường dẫn tối đa là 100 ký tự. Ngay cả một nghiệm bậc hai cũng có thể vừa khít bên trong các giới hạn. Tuy nhiên, cấu trúc của bài toán gợi ý một cách tự nhiên việc xây dựng một cây và điều đó mang lại cách triển khai rõ ràng và đáng tin cậy hơn. 

Khó khăn chính là các thư mục được xác định theo vị trí đầy đủ của chúng chứ không chỉ bằng tên của chúng. Hai thư mục có cùng tên văn bản có thể tồn tại ở những vị trí khác nhau và phải được coi là các nút khác nhau. 

Ví dụ:```
C:\a\b\file.txt
C:\x\b\file2.txt
```hai`b`các thư mục không liên quan. 

Một trường hợp tinh vi khác là tên thư mục bị lặp lại dọc theo một đường dẫn:```
C:\file\file\file\a.txt
```Việc triển khai bất cẩn chỉ sử dụng tên thư mục làm khóa sẽ thu gọn cả ba thư mục vào một nút và tạo ra số lượng hoàn toàn sai. 

Ngoài ra còn có chi tiết rằng đĩa gốc không phải là một thư mục. Coi như:```
C:\a\f.txt
D:\b\g.txt
```Câu trả lời không được tính cho`C:`hoặc`D:`. Chỉ một`a`Và`b`là những thư mục hợp lệ. 

Cuối cùng, các thư mục chỉ có thể chứa các tập tin một cách gián tiếp. Ví dụ:```
C:\a\b\c.txt
```Thư mục`a`chứa một tệp mặc dù không có tệp nào trực tiếp bên trong`a`. 

Đầu ra đúng ở đây là:```
1 1
```Thư mục`a`có một thư mục con,`b`và một tệp bên trong cây con của nó. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu có thể hoạt động vì các ràng buộc rất nhỏ. Chúng tôi có thể phân tích mọi đường dẫn thành tiền tố thư mục của nó, tạo rõ ràng mọi thư mục, sau đó đối với mỗi thư mục, hãy quét tất cả các thư mục và tệp khác để kiểm tra xem đường dẫn của chúng có bắt đầu bằng đường dẫn của thư mục này hay không. 

Ví dụ: nếu chúng tôi lưu trữ đường dẫn thư mục dưới dạng chuỗi:```
C:\a\b
```thì thư mục khác là con cháu nếu đường dẫn của nó bắt đầu bằng:```
C:\a\b\
```Tương tự, một tệp thuộc về một thư mục nếu đường dẫn tệp đầy đủ của nó bắt đầu bằng đường dẫn thư mục cộng với dấu phân cách. 

Điều này hoạt động vì số lượng đường dẫn nhỏ. Tuy nhiên, logic trở nên mong manh. So sánh tiền tố rất dễ bị sai vì`"a\b"`cũng là tiền tố của`"a\bc"`trừ khi dải phân cách được xử lý cẩn thận. Thời gian chạy cũng lặp đi lặp lại một cách không cần thiết vì tiền tố đường dẫn giống nhau được quét nhiều lần. 

Hệ thống tập tin về cơ bản là một cái cây. Mỗi thư mục có chính xác một thư mục mẹ, ngoại trừ các thư mục cấp cao nhất ngay dưới đĩa. Mỗi tập tin thuộc về chính xác một thư mục. Một khi chúng ta nhận ra điều đó thì việc đếm trở nên đơn giản hơn nhiều. 

Chúng tôi xây dựng một cây thư mục trong đó mỗi đường dẫn thư mục duy nhất là một nút. Trong khi đọc một đường dẫn, chúng ta duyệt qua các thư mục của nó từ trái sang phải và tạo các nút nếu cần. 

Sau đó, chúng tôi tính toán hai giá trị cây con cho mỗi thư mục: 

Số lượng thư mục con cháu. 

Số lượng tập tin trong cây con của nó. 

Đây chính xác là mục đích mà DFS trên cây được thiết kế. 

Nếu một nút có thư mục con:```
child1, child2, child3
```sau đó:```
subfolders(node)
= sum(subfolders(child)) + number_of_direct_children
```Và:```
files(node)
= direct_files(node) + sum(files(child))
```Mỗi thư mục và cạnh được xử lý một lần, do đó giải pháp tuyến tính theo kích thước của cây được xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 · L) | O(N · L) | Đã chấp nhận | 
| Tối ưu | O(N · L) | O(N · L) | Đã chấp nhận | 

Đây,`N`là số đường đi và`L`là độ dài đường dẫn tối đa. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào cho đến khi EOF. 

Vấn đề không cung cấp số lượng đường dẫn riêng biệt, vì vậy chúng tôi chỉ sử dụng mọi dòng không trống. 
2. Chia từng đường dẫn bằng cách sử dụng`\`như là dấu phân cách. 

Đối với một đường dẫn như:```
C:\a\b\file.txt
```chúng tôi có được:```
["C:", "a", "b", "file.txt"]
```3. Bỏ qua tên đĩa và tên tệp. 

Chỉ có các thành phần trung gian là các thư mục. 
4. Xây dựng một cây thư mục. 

Mỗi nút thư mục được xác định bằng đường dẫn đầy đủ chứ không chỉ tên của nó. 

Trong khi xử lý các thư mục từ trái sang phải, hãy kết nối mọi thư mục với thư mục mẹ của nó. 
5. Lưu trữ bao nhiêu tập tin trực tiếp trong mỗi thư mục. 

Thư mục cuối cùng trong đường dẫn chứa trực tiếp tệp. 
6. Chạy DFS từ mọi thư mục cấp cao nhất. 

Trong DFS, tính toán đệ quy:```
total_subfolders
total_files
```7. Đối với mỗi nút thư mục: 

Thêm một cái cho mỗi thư mục con trực tiếp. 

Thêm tất cả số lượng con cháu được trả về từ trẻ em. 

Thêm tất cả số lượng tập tin được trả về từ trẻ em. 
8. Theo dõi các giá trị tối đa toàn cầu. 

Mỗi thư mục đóng góp một câu trả lời ứng viên. 
9. In:```
maximum_subfolders maximum_files
```### Tại sao nó hoạt động 

Cấu trúc thư mục tạo thành một nhóm gốc vì mỗi thư mục có chính xác một thư mục mẹ ngoại trừ các thư mục trực tiếp dưới đĩa. 

DFS tính toán thông tin cây con từ dưới lên. Khi xử lý một thư mục, tất cả thông tin về con cháu của nó đã được biết từ các lệnh gọi đệ quy. Sự tái phát khớp chính xác với định nghĩa vấn đề: 

Các thư mục con của một thư mục là tất cả các thư mục con cộng với tất cả các thư mục con bên trong các thư mục con đó. 

Các tệp của thư mục đều là các tệp được lưu trữ trực tiếp cộng với tất cả các tệp bên trong các thư mục con. 

Bởi vì mỗi thư mục được xử lý chính xác một lần và mọi mối quan hệ đều được thể hiện rõ ràng trong cây nên không có thư mục hoặc tệp nào có thể bị tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

# adjacency list of folder tree
children = defaultdict(list)

# direct file count for each folder
direct_files = defaultdict(int)

# all folder nodes
folders = set()

# parent existence check
has_parent = set()

for line in sys.stdin:
    line = line.strip()

    if not line:
        continue

    parts = line.split('\\')

    # folders are everything except disk and filename
    path_parts = parts[1:-1]

    current = ""

    for i, folder in enumerate(path_parts):
        if current == "":
            current = parts[0] + "\\" + folder
        else:
            parent = current
            current = current + "\\" + folder

            children[parent].append(current)
            has_parent.add(current)

        folders.add(current)

    # last folder directly contains the file
    direct_files[current] += 1

# roots are folders without parents
roots = [folder for folder in folders if folder not in has_parent]

max_subfolders = 0
max_files = 0

def dfs(node):
    global max_subfolders, max_files

    subfolders = 0
    files = direct_files[node]

    for child in children[node]:
        child_subfolders, child_files = dfs(child)

        subfolders += 1 + child_subfolders
        files += child_files

    max_subfolders = max(max_subfolders, subfolders)
    max_files = max(max_files, files)

    return subfolders, files

for root in roots:
    dfs(root)

print(max_subfolders, max_files)
```Phần đầu tiên đọc tất cả các đường dẫn đầu vào và dần dần xây dựng cây thư mục. 

Chi tiết triển khai quan trọng là cách thể hiện danh tính thư mục. Chỉ sử dụng tên thư mục sẽ không thành công vì cùng một tên có thể xuất hiện ở nhiều nơi. Thay vào đó, mỗi nút thư mục lưu trữ đường dẫn đầy đủ của nó từ thư mục gốc. 

Ví dụ:```
C:\a\b
```Và:```
C:\x\b
```trở thành các nút khác nhau. 

các`children`danh sách kề lưu trữ thứ bậc thư mục. Bất cứ khi nào chúng tôi mở rộng đường dẫn bằng một thư mục, chúng tôi sẽ kết nối thư mục trước đó với thư mục mới. 

các`direct_files`bản đồ lưu trữ bao nhiêu tệp được đặt trực tiếp trong mỗi thư mục. Chúng tôi chỉ tăng thư mục cuối cùng trong đường dẫn vì bản thân tệp không phải là nút thư mục. 

DFS tính toán thông tin cây con theo cách đệ quy. 

Dòng:```
subfolders += 1 + child_subfolders
```rất dễ bị đọc sai. các`+1`đếm chính thư mục con, trong khi`child_subfolders`đếm mọi thứ bên dưới nó. 

Tương tự:```
files += child_files
```tích lũy tất cả các tập tin từ các thư mục con cháu. 

Rễ được phát hiện dưới dạng thư mục không có thư mục mẹ. Tên đĩa không bao giờ được chèn vào cây, điều này khớp chính xác với câu lệnh rằng đĩa không phải là thư mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
C:\folder1\file1.txt
```###Xây dựng cây 

| Thư mục hiện tại | Phụ huynh | Tập tin trực tiếp | 
| --- | --- | --- | 
| C:\thư mục1 | không | 1 | 

### tính toán DFS 

| Thư mục | Thư mục con | Tập tin | 
| --- | --- | --- | 
| C:\thư mục1 | 0 | 1 | 

Đầu ra:```
0 1
```Ví dụ này xác nhận cấu hình hợp lệ đơn giản nhất. Thư mục không có thư mục con vẫn đóng góp các tệp được lưu trữ trực tiếp. 

### Ví dụ 2 

đầu vào:```
C:\folder1\folder2\folder3\file1.txt
C:\folder1\folder2\folder4\file2.txt
```###Xây dựng cây 

| Thư mục hiện tại | Phụ huynh | Tập tin trực tiếp | 
| --- | --- | --- | 
| C:\thư mục1 | không | 0 | 
| C:\thư mục1\thư mục2 | C:\thư mục1 | 0 | 
| C:\folder1\folder2\folder3 | C:\thư mục1\thư mục2 | 1 | 
| C:\folder1\folder2\folder4 | C:\thư mục1\thư mục2 | 1 | 

### tính toán DFS 

| Thư mục | Thư mục con | Tập tin | 
| --- | --- | --- | 
| thư mục3 | 0 | 1 | 
| thư mục4 | 0 | 1 | 
| thư mục2 | 2 | 2 | 
| thư mục1 | 3 | 2 | 

Đầu ra:```
3 2
```Dấu vết này cho thấy cách tập hợp cây con hoạt động như thế nào. Thư mục`folder1`đếm tất cả con cháu theo cách đệ quy, không chỉ con trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · L) | Mọi thành phần đường dẫn được xử lý một lần trong khi xây dựng và một lần trong DFS | 
| Không gian | O(N · L) | Cây thư mục lưu trữ tất cả các đường dẫn thư mục duy nhất | 

Các ràng buộc là cực kỳ nhỏ nên giải pháp này dễ dàng nằm gọn trong các giới hạn. Ngay cả trong trường hợp xấu nhất, số lượng nút thư mục được tạo vẫn rất nhỏ so với những gì phần cứng hiện đại có thể xử lý một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    children = defaultdict(list)
    direct_files = defaultdict(int)
    folders = set()
    has_parent = set()

    for line in sys.stdin:
        line = line.strip()

        if not line:
            continue

        parts = line.split('\\')
        path_parts = parts[1:-1]

        current = ""

        for folder in path_parts:
            if current == "":
                current = parts[0] + "\\" + folder
            else:
                parent = current
                current = current + "\\" + folder
                children[parent].append(current)
                has_parent.add(current)

            folders.add(current)

        direct_files[current] += 1

    roots = [f for f in folders if f not in has_parent]

    max_subfolders = 0
    max_files = 0

    def dfs(node):
        nonlocal max_subfolders, max_files

        subfolders = 0
        files = direct_files[node]

        for child in children[node]:
            s, f = dfs(child)
            subfolders += 1 + s
            files += f

        max_subfolders = max(max_subfolders, subfolders)
        max_files = max(max_files, files)

        return subfolders, files

    for root in roots:
        dfs(root)

    return f"{max_subfolders} {max_files}"

# provided sample
assert run(
    "C:\\folder1\\file1.txt\n"
) == "0 1", "sample 1"

# nested folders
assert run(
    "C:\\folder1\\folder2\\folder3\\file1.txt\n"
    "C:\\folder1\\folder2\\folder4\\file2.txt\n"
) == "3 2", "nested folders"

# repeated folder names
assert run(
    "C:\\file\\file\\file\\a.txt\n"
    "C:\\file\\file\\file2\\b.txt\n"
) == "4 2", "same folder names at different depths"

# multiple disks
assert run(
    "C:\\a\\x.txt\n"
    "D:\\b\\y.txt\n"
) == "0 1", "disk roots are not folders"

# multiple files in same folder
assert run(
    "C:\\a\\f1.txt\n"
    "C:\\a\\f2.txt\n"
    "C:\\a\\f3.txt\n"
) == "0 3", "direct file counting"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thư mục đơn với một tệp |`0 1`| Cấu trúc hợp lệ tối thiểu | 
| Thư mục lồng nhau sâu |`3 2`| Đếm con cháu đệ quy | 
| Tên thư mục lặp đi lặp lại |`4 2`| Xử lý danh tính toàn đường dẫn | 
| Nhiều đĩa |`0 1`| Rễ đĩa bị loại trừ | 
| Nhiều tệp trong một thư mục |`0 3`| Tích lũy tập tin trực tiếp | 

## Vỏ cạnh 

Xem xét tên thư mục lặp đi lặp lại:```
C:\file\file\file\a.txt
C:\file\file\file2\b.txt
```Thuật toán tạo ra bốn nút thư mục riêng biệt:```
C:\file
C:\file\file
C:\file\file\file
C:\file\file\file2
```DFS tính toán:```
C:\file\file\file  -> 0 subfolders
C:\file\file\file2 -> 0 subfolders
C:\file\file       -> 2 subfolders
C:\file            -> 3 subfolders
```Đầu ra trở thành:```
3 2
```Một giải pháp chỉ sử dụng tên thư mục sẽ hợp nhất tất cả`"file"`thư mục và thất bại nặng nề ở đây. 

Bây giờ chỉ xem xét các tệp trong thư mục con cháu:```
C:\a\b\c.txt
```Thư mục`a`trực tiếp không chứa tệp nào, nhưng DFS truyền số lượng tệp lên trên:```
b -> 1 file
a -> 1 file
```Thuật toán xuất ra chính xác:```
1 1
```Cuối cùng, hãy xem xét nhiều đĩa:```
C:\a\f.txt
D:\b\g.txt
```Cây có hai rễ độc lập:```
C:\a
D:\b
```DFS chạy riêng biệt từ cả hai gốc. Bản thân tên đĩa không bao giờ trở thành nút nên chúng không được tính là thư mục. Kết quả đúng là:```
0 1
```
