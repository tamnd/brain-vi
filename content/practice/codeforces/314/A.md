---
title: "CF 314A - Sereja và cuộc thi"
description: "Những người tham gia được liệt kê theo thứ tự xếp hạng cuối cùng. Người tham gia tôi có xếp hạng a[i]. Đối với một bảng có kích thước m, nếu một người tham gia hiện đang chiếm vị trí p, thì thay đổi xếp hạng của anh ta là $$d = sum{text{những người tham gia trước anh ta}} aj (posj-1) - ai (p-1)(m-p)$$ Trong tuyên bố ban đầu này…"
date: "2026-06-06T01:11:10+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 314
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 187 (Div. 1)"
rating: 1600
weight: 314
solve_time_s: 144
verified: true
draft: false
---

[CF 314A - Sereja và Cuộc thi](https://codeforces.com/problemset/problem/314/A) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Những người tham gia được liệt kê theo thứ tự xếp hạng cuối cùng. Người tham gia`i`có đánh giá`a[i]`. 

Đối với một bảng có kích thước`m`, nếu một người tham gia hiện đang chiếm giữ vị trí`p`, sự thay đổi xếp hạng của anh ấy là$$d = \sum_{\text{participants before him}} a_j (pos_j-1)     - a_i (p-1)(m-p)$$Trong tuyên bố ban đầu, công thức này được đưa ra trực tiếp. Điều phức tạp là bảng rất năng động. 

Bất cứ khi nào tồn tại một người tham gia với`d < k`, người tham gia có thứ hạng hiện tại tốt nhất trong số tất cả những người tham gia đó sẽ bị loại. Sau khi loại bỏ, các vị trí được nén và tất cả`d`các giá trị được tính toán lại. Chúng ta phải xuất ra các chỉ mục ban đầu của tất cả những người tham gia đã bị loại bỏ, theo thứ tự chúng biến mất. 

Ràng buộc`n ≤ 2·10^5`ngay lập tức loại trừ bất kỳ mô phỏng nào liên tục tính toán lại tất cả các thay đổi xếp hạng. Thậm chí một`O(n^2)`giải pháp sẽ thực hiện xung quanh`4·10^10`hoạt động trong trường hợp xấu nhất. 

Phần khó khăn là việc xóa sẽ thay đổi vị trí, do đó giá trị của người tham gia`d`không cố định. Việc triển khai đơn giản có thể dễ dàng loại bỏ người tham gia sai vì nó đánh giá`d`sử dụng các vị trí lỗi thời. 

Coi như:```
5 0
5 3 4 1 2
```Người tham gia 2 bị loại trước. Sau đó, người tham gia thứ 3 tiến về phía trước và thay đổi xếp hạng của anh ta phải được tính toán lại bằng cách sử dụng các vị trí mới. Việc sử dụng toàn bộ bảng gốc sẽ tạo ra trình tự sai. 

Một trường hợp tinh vi khác xảy ra khi một người tham gia hiện đang an toàn.```
3 0
10 1 10
```Nếu người tham gia 2 không bị loại bỏ khi chúng tôi liên hệ với anh ta, thì những lần loại bỏ sau đó không thể khiến anh ta bị loại bỏ.`d`nhỏ hơn. Một giải pháp liên tục xem lại những người tham gia trước đó là làm những công việc không cần thiết. 

Giá trị của`k`cũng có thể tiêu cực. Một người tham gia có ý kiến ​​tiêu cực`d`không được tự động loại bỏ. Điều kiện là nghiêm ngặt`d < k`. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu tuân theo tuyên bố theo đúng nghĩa đen. Liên tục tính toán hiện tại của mỗi người tham gia`d`, tìm cái đầu tiên với`d < k`, loại bỏ anh ta, nén bảng và lặp lại. 

Điều này đúng vì nó tái tạo chính xác quá trình. Thật không may, một lần tính toán lại đã tốn kém`O(n)`, và có thể có`O(n)`việc gỡ bỏ. Tổng độ phức tạp trở thành`O(n^2)`, quá chậm đối với`n = 200000`. 

Quan sát quan trọng là việc loại bỏ luôn xảy ra từ trái sang phải theo chỉ số ban đầu. 

Giả sử người tham gia`i`hiện đang sống sót. Nếu một số người tham gia sau anh ta bị loại bỏ thì số người sau đó`i`giảm đi. Nhìn vào công thức,$$d_i = \text{constant} - a_i (p_i-1)(m-p_i),$$yếu tố`(m-p_i)`trở nên nhỏ hơn. Vì nó nhân với dấu trừ nên`d_i`chỉ có thể tăng lên. 

Vì vậy một khi người tham gia`i`đạt tới trạng thái ở đó`d_i ≥ k`, những lần xóa trong tương lai không bao giờ có thể khiến anh ấy đủ điều kiện trở lại. 

Điều đó có nghĩa là khi chúng tôi quét người tham gia từ trái sang phải, quyết định dành cho người tham gia`i`chỉ phụ thuộc vào việc xóa đã xảy ra trước đó`i`. Nếu như`d_i < k`, anh ta phải bị loại bỏ ngay lập tức. Nếu như`d_i ≥ k`, anh ấy sẽ tồn tại mãi mãi. 

Điều này biến quá trình động thành một lần quét tuyến tính duy nhất. 

Cho phép`r`là số lượng người tham gia bị loại bỏ trước chỉ mục hiện tại`i`. 

Vị trí hiện tại của người tham gia`i`là$$p = i-r.$$Kích thước bảng hiện tại là$$m = n-r.$$Trong số những người tham gia còn sống sót trước`i`, các vị trí chính xác`1,2,\dots,p-1`. 

Nếu chúng ta duy trì$$S=\sum_{\text{surviving } j<i} a_j (pos_j-1),$$sau đó$$d_i = S - a_i (p-1)(m-p).$$Mọi thứ cần thiết để đánh giá`d_i`có sẵn ở`O(1)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`removed = 0`. 
2. Duy trì`S`, tổng của`a_j * (current_position_j - 1)`trên tất cả những người tham gia còn sống đã được xử lý. 
3. Xử lý người tham gia từ trái sang phải theo thứ tự ban đầu. 
4. Đối với người tham gia`i`, tính vị trí hiện tại của anh ta:$$p = i - removed.$$Kích thước bảng hiện tại là:$$m = n - removed.$$5. Tính toán$$d = S - a_i (p-1)(m-p).$$Đây chính xác là công thức thay đổi xếp hạng được áp dụng cho bảng động hiện tại. 
6. Nếu`d < k`, hãy xóa người tham gia này. 

Tăng`removed`và lưu trữ chỉ mục ban đầu của anh ấy trong câu trả lời. 

Không thêm bất cứ điều gì vào`S`, bởi vì những người tham gia đã bị loại bỏ sẽ không có mặt trong các tính toán sau này. 
7. Nếu không thì người tham gia sẽ sống sót. 

Thêm vào$$a_i (p-1)$$ĐẾN`S`, bởi vì người tham gia này đóng góp vào số hạng đầu tiên của mỗi công thức của người sống sót sau này. 
8. Tiếp tục cho đến khi tất cả người tham gia đã được xử lý. 

### Tại sao nó hoạt động 

Khi một người tham gia sau chỉ mục`i`bị loại bỏ, giá trị`(m-p_i)`giảm trong khi mọi thứ khác trong`d_i`vẫn không thay đổi. Kết quả là,`d_i`không bao giờ giảm. 

Vì vậy nếu người tham gia`i`thỏa mãn`d_i ≥ k`tại thời điểm chúng tôi xử lý anh ấy, sau này anh ấy sẽ không bao giờ có thể tháo rời được. Ngược lại, nếu`d_i < k`, các quy tắc yêu cầu loại bỏ anh ta ngay lập tức vì mọi người tham gia có thể loại bỏ trước anh ta đều đã được xử lý. 

Do đó, quá trình quét tái tạo chính xác trình tự xóa giống như quy trình động được mô tả trong câu lệnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    removed = 0
    s = 0
    ans = []

    for i in range(1, n + 1):
        ai = a[i - 1]

        p = i - removed
        m = n - removed

        d = s - ai * (p - 1) * (m - p)

        if d < k:
            removed += 1
            ans.append(str(i))
        else:
            s += ai * (p - 1)

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`removed`theo dõi có bao nhiêu người tham gia trước đó đã biến mất. Từ con số duy nhất đó chúng ta có thể khôi phục cả vị trí hiện tại`p`và kích thước bảng hiện tại`m`. 

Biến`s`lưu trữ số hạng đầu tiên của công thức. Chỉ những người tham gia còn sống mới đóng góp vào nó. Khi một người tham gia bị xóa, bỏ qua cập nhật`s`là điều cần thiết. Việc thêm đóng góp của anh ấy sẽ cho phép những người tham gia bị loại bỏ ảnh hưởng đến các tính toán sau này một cách không chính xác. 

Tất cả số học được thực hiện với số nguyên Python. Giá trị trung gian có thể đạt tới khoảng$$10^9 \cdot 2\cdot10^5 \cdot 2\cdot10^5,$$vượt xa phạm vi 32-bit. Python tự động xử lý việc này. 

Thứ tự thực hiện các thao tác cũng rất quan trọng. Chúng ta phải đánh giá`d`trước khi quyết định liệu người tham gia có sống sót hay không. Đang cập nhật`s`đầu tiên sẽ cho phép người tham gia đóng góp vào tính toán thay đổi xếp hạng của chính mình một cách không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 0
5 3 4 1 2
```| tôi | ai | đã xóa trước | p | m | S trước | d | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 1 | 5 | 0 | 0 | giữ | 
| 2 | 3 | 0 | 2 | 5 | 0 | -9 | xóa | 
| 3 | 4 | 1 | 2 | 4 | 0 | -8 | xóa | 
| 4 | 1 | 2 | 2 | 3 | 0 | -1 | xóa | 
| 5 | 2 | 3 | 2 | 2 | 0 | 0 | giữ | 

Đầu ra:```
2
3
4
```Ví dụ này cho thấy ý tưởng cốt lõi. Sau mỗi lần xóa, những người tham gia sau sẽ tiến về phía trước và quá trình quét sẽ tự động tính đến điều đó thông qua giá trị được cập nhật của`removed`. 

### Mẫu 2 

đầu vào:```
10 -10
5 5 1 7 5 1 2 4 9 2
```| tôi | đã xóa trước | p | m | d | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 10 | 0 | giữ | 
| 2 | 0 | 2 | 10 | -40 | xóa | 
| 3 | 1 | 2 | 9 | -8 | giữ | 
| 4 | 1 | 3 | 9 | -86 | xóa | 
| 5 | 2 | 3 | 8 | -44 | xóa | 
| 6 | 3 | 3 | 7 | 2 | giữ | 
| 7 | 3 | 4 | 7 | -11 | xóa | 
| 8 | 4 | 4 | 6 | -12 | xóa | 
| 9 | 5 | 4 | 5 | -27 | xóa | 
| 10 | 6 | 4 | 4 | 16 | giữ | 

Đầu ra:```
2
4
5
7
8
9
```Dấu vết này chứng tỏ rằng việc loại bỏ xảy ra theo thứ tự chỉ mục ban đầu tăng dần. Khi một người tham gia sống sót đến lượt của mình, chúng tôi không bao giờ cần phải quay lại thăm anh ta. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần duyệt qua mảng, O(1) công việc cho mỗi người tham gia | 
| Không gian | O(n) | Danh sách đầu ra của các chỉ số bị loại bỏ | 

Với`n = 200000`, quét tuyến tính dễ dàng đủ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ bị chi phối bởi mảng câu trả lời và vẫn nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    removed = 0
    s = 0
    ans = []

    for i in range(1, n + 1):
        ai = a[i - 1]

        p = i - removed
        m = n - removed

        d = s - ai * (p - 1) * (m - p)

        if d < k:
            removed += 1
            ans.append(str(i))
        else:
            s += ai * (p - 1)

    return "\n".join(ans)

# provided samples
assert run("5 0\n5 3 4 1 2\n") == "2\n3\n4", "sample 1"

assert run("10 -10\n5 5 1 7 5 1 2 4 9 2\n") == "2\n4\n5\n7\n8\n9", "sample 2"

# minimum size
assert run("1 0\n100\n") == "", "single participant"

# all equal values
assert run("4 0\n1 1 1 1\n") == "2\n3", "equal ratings"

# very negative threshold, nobody removed
assert run("5 -1000000000\n1 2 3 4 5\n") == "", "no removals"

# catches position-update mistakes
assert run("3 0\n5 1 1\n") == "2", "dynamic position handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 100`| đầu ra trống | Kích thước tối thiểu | 
|`4 0 / 1 1 1 1`|`2 3`| Giá trị bằng nhau và xóa nhiều lần | 
|`5 -1000000000 / 1 2 3 4 5`| đầu ra trống | Không ai đủ điều kiện để loại bỏ | 
|`3 0 / 5 1 1`|`2`| Cập nhật vị trí chính xác sau khi xóa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 0
100
```Người tham gia duy nhất có`p = 1`, Vì thế`(p-1) = 0`Và`d = 0`. Từ`0 < 0`là sai, không ai bị loại bỏ. Thuật toán không đưa ra kết quả gì, điều này đúng. 

Hãy xem xét trường hợp một người tham gia sống sót và sau đó bị loại bỏ:```
4 0
10 1 10 1
```Người tham gia 3 có thể trông nguy hiểm sau khi người tham gia 4 biến mất, nhưng việc loại bỏ những người đứng sau anh ta chỉ giảm bớt mà thôi`(m-p)`. Của anh ấy`d`chỉ có thể tăng lên. Thuật toán xử lý anh ta một lần và không bao giờ truy cập lại anh ta, điều này phù hợp với hành vi toán học của công thức. 

Hãy xem xét một mảng hoàn toàn bằng nhau:```
4 0
1 1 1 1
```Quá trình quét sẽ tính toán vị trí hiện tại sau mỗi lần xóa bằng cách sử dụng`p = i - removed`. Đây chính xác là nơi nhiều giải pháp không chính xác thất bại. Sử dụng vị trí ban đầu sẽ tạo ra sai lệch`d`giá trị sau lần xóa đầu tiên. Việc duy trì`removed`bộ đếm đảm bảo rằng các vị trí nén luôn chính xác.
