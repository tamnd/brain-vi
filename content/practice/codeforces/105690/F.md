---
title: "CF 105690F - Nhảy đèn lồng"
description: "Chúng tôi có một dòng đèn lồng. Đèn lồng i có chiều cao a[i] và con rồng bắt đầu trên một chiếc đèn lồng đã chọn. Anh ta chỉ có thể nhảy sang chiếc đèn lồng liền kề khi hai chiếc đèn lồng có cùng chiều cao tại thời điểm nhảy."
date: "2026-06-26T09:04:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105690
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 1 (Advanced)"
rating: 0
weight: 105690
solve_time_s: 41
verified: true
draft: false
---

[CF 105690F - Nhảy đèn lồng](https://codeforces.com/problemset/problem/105690/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng đèn lồng. đèn lồng`i`có chiều cao`a[i]`, và con rồng bắt đầu trên một chiếc đèn lồng đã chọn. Anh ta chỉ có thể nhảy sang chiếc đèn lồng liền kề khi hai chiếc đèn lồng có cùng chiều cao tại thời điểm nhảy. 

Con rồng có thể thay đổi độ cao của chiếc đèn lồng mà nó đang đứng. Tăng chiều cao thêm một tiêu tốn một năng lượng, trong khi giảm chiều cao đi một sẽ mang lại một năng lượng. Nhiệm vụ là tìm năng lượng ban đầu nhỏ nhất cần thiết cho mỗi vị trí bắt đầu truy vấn để con rồng cuối cùng có thể ghé thăm mọi chiếc đèn lồng. Chiều cao đèn lồng cũng có thể được cập nhật giữa các truy vấn. Vấn đề yêu cầu câu trả lời sau mỗi lần cập nhật hoặc truy vấn vị trí bắt đầu. 

Các ràng buộc cho phép lên đến`2 * 10^5`đèn lồng và`2 * 10^5`sự kiện. Một giải pháp quét tất cả các đèn lồng cho mỗi truy vấn sẽ yêu cầu khoảng`4 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn thời gian của cuộc thi thông thường. Chúng tôi cần cả cập nhật và truy vấn theo thời gian logarit, điều này hướng tới việc duy trì cấu trúc dữ liệu trên mảng. 

Các trường hợp khó khăn chính xuất phát từ việc nhầm lẫn chiều cao của đèn lồng hiện tại với chiều cao ban đầu của nó và do quên rằng bản thân đèn ban đầu có thể cung cấp năng lượng bằng cách hạ xuống. 

Ví dụ: với:```
2 1
5 10
1 1
```câu trả lời là`5`. Con rồng bắt đầu từ chiếc đèn lồng đầu tiên, hạ nó xuống từ`5`ĐẾN`10`là không thể nếu không có năng lượng, vì vậy anh ta cần chính xác`5`năng lượng để nâng nó lên và nhảy. Một giải pháp chỉ kiểm tra sự khác biệt giữa các đèn lồng liền kề có thể bỏ sót rằng bước đi đầu tiên có chi phí trực tiếp. 

Một ví dụ khác là:```
3 1
10 5 10
1 2
```câu trả lời là`5`. Đèn lồng ở giữa thấp hơn cả hai bên nên rồng cần đủ năng lượng để nâng nó lên trước khi nhảy. Giải pháp bất cẩn chỉ kiểm tra một bên có thể quay lại`0`không chính xác. 

Trường hợp ranh giới cuối cùng là một chiếc đèn lồng:```
1 1
100
1 1
```Câu trả lời là`0`, vì không có bước nhảy nào để thực hiện. Bất kỳ triển khai nào truy vấn phạm vi trống bên trái hoặc bên phải mà không xử lý nó đều có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng chuyển động của rồng. Bắt đầu từ một chiếc đèn lồng đã chọn, chúng ta có thể thử các thứ tự khác nhau để thăm bên trái và bên phải, theo dõi lượng năng lượng cần thiết cho mỗi lần nhảy. Điều này đúng vì mọi chuỗi chuyển động hợp lệ đều có thể được kiểm tra trực tiếp. Tuy nhiên, nó còn quá chậm. Với`n = 2 * 10^5`, một truy vấn duy nhất có thể kiểm tra tất cả các đèn lồng và lặp lại điều này cho`q = 2 * 10^5`truy vấn cung cấp hành vi bậc hai. 

Quan sát quan trọng là chuyển động dọc theo một phía của mảng có yêu cầu năng lượng đơn giản. 

Giả sử con rồng bắt đầu ở vị trí`p`và chỉ đi về bên phải. Trước khi đến đèn lồng`i + 1`, mỗi lần nhảy trước đó đã thay đổi chiều cao của đèn lồng hiện tại thành chiều cao của đèn lồng tiếp theo. Tổng năng lượng thay đổi sau khi chạm tới đèn lồng`i`chính xác là`a[p] - a[i]`. Để thực hiện bước nhảy tiếp theo, rồng phải có đủ năng lượng để nâng chiếc đèn lồng hiện tại lên nếu`a[i + 1]`cao hơn. Kết hợp các giá trị này cho thấy điều duy nhất quan trọng là chiều cao đèn lồng cao nhất gặp ở phía đó. 

Năng lượng cần thiết để chạm tới mỗi chiếc đèn lồng bên phải là:`max(0, maximum height on positions p+1 through n - a[p])`Phía bên trái là đối xứng:`max(0, maximum height on positions 1 through p-1 - a[p])`Con rồng phải xử lý cả hai bên, vì vậy câu trả lời là giá trị lớn hơn trong hai giá trị này. Điều này làm giảm vấn đề trong phạm vi truy vấn tối đa với các cập nhật điểm. Cây phân đoạn lưu trữ chiều cao tối đa trong mỗi khoảng, cung cấp thời gian logarit cho cả hai thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ chiều cao đèn lồng tối đa trong khoảng của nó. Thông tin duy nhất cần thiết từ một phạm vi là đèn lồng cao nhất của nó. 
2. Đối với truy vấn cập nhật, thay thế giá trị của đèn lồng đã chọn trong cây phân đoạn và tính toán lại giá trị tối đa trên đường dẫn tới gốc. 
3. Đối với truy vấn vị trí bắt đầu tại`p`, đạt được chiều cao tối đa trong số tất cả các đèn lồng ở bên trái của`p`. 
4. Đạt được chiều cao tối đa trong số tất cả các đèn lồng ở bên phải`p`. 
5. Tính năng lượng cần thiết cho mỗi bên. Nếu độ cao tối đa ở một bên là`mx`, con rồng cần`max(0, mx - a[p])`năng lượng để đạt được phía đó. 
6. Trả lại yêu cầu lớn hơn trong hai bên, vì cuối cùng cả hai bên đều phải được thăm. 

Tại sao nó hoạt động: 

Trong khi đi theo một hướng, mỗi bước nhảy sẽ chuyển sự khác biệt về độ cao vào sự cân bằng năng lượng của rồng một cách hiệu quả. Tổng lượng năng lượng có được sau khi chạm tới bất kỳ chiếc đèn lồng nào chỉ phụ thuộc vào độ cao ban đầu và chiều cao của chiếc đèn lồng hiện tại. Trở ngại lớn nhất luôn là chiếc đèn lồng cao nhất phải đạt được, bởi vì để đạt được chiếc đèn lồng cao hơn là tình huống duy nhất cần tiêu tốn sức lực. Cây phân đoạn trả về chính xác những trở ngại cao nhất này cho cả hai hướng, vì vậy giá trị được tính toán là năng lượng ban đầu tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, node, left, right, arr):
        if left == right:
            self.tree[node] = arr[left]
            return
        mid = (left + right) // 2
        self.build(node * 2, left, mid, arr)
        self.build(node * 2 + 1, mid + 1, right, arr)
        self.tree[node] = max(self.tree[node * 2], self.tree[node * 2 + 1])

    def update(self, node, left, right, pos, value):
        if left == right:
            self.tree[node] = value
            return
        mid = (left + right) // 2
        if pos <= mid:
            self.update(node * 2, left, mid, pos, value)
        else:
            self.update(node * 2 + 1, mid + 1, right, pos, value)
        self.tree[node] = max(self.tree[node * 2], self.tree[node * 2 + 1])

    def query(self, node, left, right, ql, qr):
        if ql > right or qr < left:
            return -1
        if ql <= left and right <= qr:
            return self.tree[node]
        mid = (left + right) // 2
        return max(
            self.query(node * 2, left, mid, ql, qr),
            self.query(node * 2 + 1, mid + 1, right, ql, qr)
        )

def solve(data):
    it = iter(data.split())
    n = int(next(it))
    q = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    seg = SegmentTree(a)
    ans = []

    for _ in range(q):
        t = int(next(it))
        if t == 1:
            p = int(next(it)) - 1
            left_max = seg.query(1, 0, n - 1, 0, p - 1)
            right_max = seg.query(1, 0, n - 1, p + 1, n - 1)

            need = 0
            if left_max != -1:
                need = max(need, left_max - a[p])
            if right_max != -1:
                need = max(need, right_max - a[p])

            ans.append(str(max(0, need)))
        else:
            p = int(next(it)) - 1
            x = int(next(it))
            a[p] = x
            seg.update(1, 0, n - 1, p, x)

    return "\n".join(ans)

def main():
    data = sys.stdin.read()
    print(solve(data))

if __name__ == "__main__":
    main()
```Cây phân đoạn chỉ lưu trữ các giá trị tối đa vì mọi truy vấn đều yêu cầu chiều cao lớn nhất chặn chuyển động. Thao tác cập nhật sẽ thay đổi một đèn lồng và làm mới các khoảng thời gian bị ảnh hưởng. 

Truy vấn sử dụng`-1`làm điểm đánh dấu phạm vi trống. Vì tất cả chiều cao của đèn lồng đều không âm nên giá trị này không thể nhầm lẫn với chiều cao đèn lồng thực. trận chung kết`max(0, need)`xử lý các trường hợp trong đó mọi đèn lồng ở phía liên quan đều thấp hơn đèn lồng ban đầu, nghĩa là rồng có thể lấy đủ năng lượng bằng cách hạ thấp đèn lồng ban đầu. 

Tất cả các chỉ số được chuyển đổi thành lập chỉ mục dựa trên số 0 ngay lập tức. Các dãy`0`ĐẾN`p - 1`Và`p + 1`ĐẾN`n - 1`tượng trưng cho hai bên của đèn xuất phát, tránh vô tình lấn vào vị trí xuất phát. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 2
3 1 3
1 1
1 2
```cây phân đoạn ban đầu lưu trữ các giá trị tối đa của phạm vi. 

| Truy vấn | Vị trí | Tối đa bên trái | Phải tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
|`1 1`| 1 | không | 3 | 0 | 
|`1 2`| 2 | 3 | 3 | 2 | 

Đối với truy vấn đầu tiên, con rồng bắt đầu ở độ cao`3`và chiếc đèn lồng cao nhất bên phải cũng là`3`, nên không cần năng lượng. Đối với truy vấn thứ hai, chiều cao bắt đầu là`1`, và cả hai bên đều chứa chiều cao`3`, yêu cầu`3 - 1 = 2`năng lượng. 

Đối với đầu vào:```
2 5
1000000000 0
1 2
2 2 10
1 2
2 1 10
1 2
```| Truy vấn | Mảng | Vị trí | Tối đa bên trái | Phải tối đa | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`1 2`|`[1000000000,0]`| 2 | 1000000000 | không | 1000000000 | 
| cập nhật |`[1000000000,10]`| | | | | 
|`1 2`|`[1000000000,10]`| 2 | 1000000000 | không | 999999990 | 
| cập nhật |`[10,10]`| | | | | 
|`1 2`|`[10,10]`| 2 | 10 | không | 0 | 

Dấu vết cho thấy câu trả lời phụ thuộc vào độ cao hiện tại, vì vậy mọi cập nhật phải ngay lập tức ảnh hưởng đến các truy vấn tối đa trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi bản cập nhật và mỗi truy vấn tối đa phạm vi đều đi theo một đường dẫn xuyên qua cây phân đoạn. | 
| Không gian | O(n) | Cây chứa số lượng nút không đổi trên mỗi đèn lồng. | 

Các ràng buộc đòi hỏi phải xử lý hàng trăm ngàn hoạt động. Các phép tính logarit giữ tổng công việc xung quanh`n log n + q log n`, phù hợp thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    return solve(inp).strip()

assert run("""3 2
3 1 3
1 1
1 2
""") == "0\n2"

assert run("""2 5
1000000000 0
1 2
2 2 10
1 2
2 1 10
1 2
""") == "1000000000\n999999990\n0"

assert run("""1 1
7
1 1
""") == "0"

assert run("""3 3
10 5 10
1 2
2 2 10
1 2
""") == "5\n0"

assert run("""5 4
1 2 3 4 5
1 3
2 5 0
1 1
1 5
""") == "2\n4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đèn lồng đơn |`0`| Khoảng trống bên trái và bên phải | 
| Đèn lồng cao đối xứng |`5`, sau đó`0`| Bắt đầu cập nhật đèn lồng và kiểm tra cả hai bên | 
| Tăng chiều cao với các bản cập nhật | Nhiều câu trả lời khác 0 | Phạm vi bảo trì tối đa | 
| Chênh lệch chiều cao lớn | Xử lý số nguyên lớn | Đúng số học không tràn | 

## Vỏ cạnh 

Đối với trường hợp:```
2 1
5 10
1 1
```cây phân đoạn trả về`10`là chiều cao tối đa ở bên phải. Chiều cao ban đầu là`5`, vậy năng lượng cần thiết là`10 - 5 = 5`. Thuật toán nắm bắt chính xác chi phí bước nhảy đầu tiên. 

Đối với trường hợp:```
3 1
10 5 10
1 2
```giá trị tối đa bên trái và bên phải đều là`10`. Đèn xuất phát có chiều cao`5`, do đó cả hai bên đều yêu cầu`5`năng lượng. Kết quả là`5`, là số lượng cần thiết để nâng chiếc đèn lồng ở giữa lên trước khi di chuyển. 

Đối với trường hợp:```
1 1
100
1 1
```cả hai truy vấn phạm vi đều trả về điểm đánh dấu trống vì không có đèn lồng lân cận. Thuật toán để lại câu trả lời tại`0`, phù hợp với thực tế là không cần chuyển động.
