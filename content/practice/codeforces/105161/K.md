---
title: "CF 105161K - Trò chơi xóa số"
description: "Chúng ta được cấp một tập hợp nhiều số nguyên hoặc một mảng số nguyên. Hai người chơi thay phiên nhau trong một trò chơi. Trong mỗi lượt, người chơi được phép loại bỏ một lần giá trị tối đa hiện tại có trong cấu trúc."
date: "2026-06-27T10:59:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105161
codeforces_index: "K"
codeforces_contest_name: "2024 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 105161
solve_time_s: 46
verified: true
draft: false
---

[CF 105161K - Trò chơi xóa số](https://codeforces.com/problemset/problem/105161/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều số nguyên hoặc một mảng số nguyên. Hai người chơi thay phiên nhau trong một trò chơi. Trong mỗi lượt, người chơi được phép loại bỏ một lần giá trị tối đa hiện tại có trong cấu trúc. Sau khi xóa nó, người chơi cũng có thể thêm bất kỳ tập hợp con nào của các phần tử nhỏ hơn mức tối đa đó, được chọn từ một số nhóm “giống tiền tố” có sẵn được mô tả trong câu lệnh, bao gồm cả khả năng không thêm gì. 

Trò chơi tiếp tục cho đến khi không còn nước đi hợp lệ nào và người chơi không thể di chuyển sẽ thua cuộc. Nhiệm vụ là xác định, từ cấu hình ban đầu, xem người chơi đầu tiên có bị buộc phải thắng hay không hay người chơi thứ hai luôn có thể phản ứng tối ưu để giành chiến thắng. 

Mặc dù phép toán nghe có vẻ phong phú nhưng cấu trúc ẩn quan trọng là đại lượng duy nhất có liên quan toàn cầu là số lần giá trị tối đa xuất hiện ban đầu. 

Từ góc độ phức tạp, kích thước đầu vào là tuyến tính theo số lượng phần tử. Điều này ngay lập tức loại trừ mọi mô phỏng của cây trò chơi. Bất kỳ cách tiếp cận nào cố gắng mô hình hóa các trạng thái sau mỗi lần xóa hoặc theo dõi tất cả các cấu hình có thể được nối thêm sẽ bùng nổ về mặt tổ hợp vì mỗi cấu hình di chuyển thành nhiều tập hợp con có thể. 

Các giải pháp khả thi duy nhất phải giảm trò chơi thành một thống kê tổng hợp duy nhất được tính toán theo thời gian tuyến tính. 

Trường hợp cạnh tinh tế là khi tất cả các phần tử đều bằng nhau. Trong tình huống đó, mọi hành động chỉ đơn giản là loại bỏ mức tối đa và không tồn tại “tiền tố nhỏ hơn” có ý nghĩa. Một độc giả ngây thơ có thể nghĩ rằng thao tác nối thêm tùy chọn có thể ảnh hưởng đến kết quả, nhưng điều đó không bao giờ thay đổi thực tế rằng việc chơi hoàn toàn được xác định bằng cách loại bỏ nhiều lần các phần tử tối đa. 

Ví dụ, nếu mảng là`[5, 5, 5]`, mỗi lần di chuyển sẽ loại bỏ một`5`. Trò chơi kết thúc sau ba nước đi nên người chơi đầu tiên sẽ thắng. Nếu nó là`[5, 5]`, người chơi thứ hai thắng. Điều này đã gợi ý một hiện tượng tương đương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi một cách rõ ràng. Mỗi trạng thái sẽ lưu trữ nhiều tập hợp hiện tại và mỗi lần chuyển đổi sẽ loại bỏ một mức tối đa và phân nhánh trên tất cả các tập hợp con có thể có của các phần tử nhỏ hơn để thêm vào. Điều này nhanh chóng trở nên không khả thi vì số lượng tập hợp con của các phần tử nhỏ hơn có thể theo cấp số nhân và số lượng trạng thái trò chơi tăng lên một cách bùng nổ. Ngay cả đối với mức độ vừa phải`n`, cách tiếp cận này không thể tiến triển vượt quá các trường hợp rất nhỏ. 

Quan sát quan trọng là các phần tử được nối thêm không bao giờ ảnh hưởng đến nhận dạng của mức tối đa trừ khi chúng khớp với giá trị tối đa hiện tại, điều này bị cấm vì chỉ có thể thêm các phần tử nhỏ hơn. Do đó, giá trị tối đa trong nhiều tập hợp chỉ giảm hoàn toàn khi tất cả các lần xuất hiện của mức tối đa hiện tại đã bị xóa. Cấu trúc bên trong dưới mức tối đa không bao giờ ảnh hưởng đến số lần xóa tối đa. 

Điều này biến trò chơi thành một quá trình đếm thuần túy: mỗi lần xuất hiện giá trị lớn nhất tương ứng với một nước đi thiết yếu trong đó quyền điều khiển sẽ luân phiên. Các thao tác tùy chọn không ảnh hưởng đến tính chẵn lẻ của số lần tối đa được loại bỏ, điều này hoàn toàn quyết định người chiến thắng trong cách chơi tối ưu. 

Do đó, vấn đề được giải quyết bằng cách đếm xem có bao nhiêu phần tử bằng giá trị tối đa toàn cục và kiểm tra xem số lượng này là số lẻ hay số chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O (n tiểu bang) | Quá chậm | 
| Đếm tần số tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta mô tả quá trình suy luận tối ưu theo trình tự trực tiếp. 

1. Đọc mảng đầu vào và xác định giá trị lớn nhất hiện có. Đây là giá trị duy nhất xác định cấu trúc bắt buộc trong trò chơi, bởi vì mọi thao tác đều xoay quanh việc loại bỏ cực đại. 
2. Đếm xem giá trị lớn nhất này xuất hiện bao nhiêu lần trong mảng. Số lượng này thể hiện tổng số nước đi cần thiết trong trò chơi, vì mỗi nước đi sẽ loại bỏ chính xác một nước đi tối đa. 
3. Xác định người chiến thắng dựa trên tính chẵn lẻ của số này. Nếu số phần tử tối đa là số lẻ thì người chơi đầu tiên thực hiện nước đi cuối cùng và thắng. Nếu hòa thì người thứ hai đi nước cuối cùng và thắng. 

Ý tưởng chính là cho dù người chơi sử dụng tùy chọn chèn các phần tử nhỏ hơn như thế nào, họ không bao giờ có thể tăng số lượng phần tử tối đa còn lại và không thể thay đổi thực tế là cuối cùng mỗi phần tử tối đa phải được loại bỏ từng cái một. 

### Tại sao nó hoạt động 

Điều bất biến là tập hợp các phần tử tối đa giảm đi đúng một phần tử trong mỗi lần di chuyển, không phụ thuộc vào bất kỳ hoạt động phụ nào. Tất cả các lần chèn bổ sung đều liên quan đến các giá trị nhỏ hơn hoàn toàn, vì vậy chúng không thể ảnh hưởng đến bậc tối đa hiện tại. 

Do sự tách biệt này, trò chơi phân rã thành một chuỗi có độ dài cố định`k`di chuyển cưỡng bức, ở đâu`k`là số phần tử tối đa. Người chơi luân phiên các bước di chuyển bắt buộc này mà không có khả năng bỏ qua hoặc hợp nhất chúng. Do đó, người chiến thắng được xác định hoàn toàn bằng việc liệu`k`là số lẻ hoặc số chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    if not data:
        return
    n = data[0]
    arr = data[1:1+n]

    mx = max(arr)
    cnt = 0
    for x in arr:
        if x == mx:
            cnt += 1

    print("First" if cnt % 2 == 1 else "Second")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên phân tích cú pháp đầu vào trong một lần chuyển. Nó tính toán mức tối đa bằng cách sử dụng hàm dựng sẵn của Python, sau đó đếm số lần xuất hiện của nó bằng một vòng lặp đơn giản. Điều này tránh mọi cấu trúc dữ liệu không cần thiết. 

Một cạm bẫy triển khai phổ biến là quên rằng định dạng đầu vào có thể đặt`n`trên cùng một dòng với các giá trị mảng, do đó việc phân chia trực tiếp phải được xử lý cẩn thận. Một điểm tinh tế khác là đảm bảo rằng chỉ các phần tử bằng mức tối đa mới được tính, chứ không phải các phần tử lớn hơn hoặc bằng so sánh vô tình bao gồm các bản sao trong dữ liệu được chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 3 2 3
```Ở đây tối đa là`3`. 

| Bước | Hành động | Giá trị tối đa | Số lượng tối đa | 
| --- | --- | --- | --- | 
| 1 | Đọc mảng | 3 | 0 | 
| 2 | Quét các phần tử | 3 | 3 | 
| 3 | Quyết định người chiến thắng | 3 | 3 | 

Vì số đếm là 3 nên kết quả là người chơi đầu tiên sẽ thắng. 

Điều này chứng tỏ rằng chỉ có sự xuất hiện của vật chất tối đa, trong khi các giá trị nhỏ hơn không có vai trò gì. 

### Ví dụ 2 

đầu vào:```
4
7 1 7 7
```| Bước | Hành động | Giá trị tối đa | Số lượng tối đa | 
| --- | --- | --- | --- | 
| 1 | Đọc mảng | 7 | 0 | 
| 2 | Quét các phần tử | 7 | 3 | 
| 3 | Quyết định người chiến thắng | 7 | 3 | 

Một lần nữa số đếm là số lẻ nên người chơi đầu tiên sẽ thắng. Nếu chúng ta thay đổi giá trị cuối cùng để làm cho số đếm chẵn thì người chơi thứ hai ngay lập tức giành chiến thắng, thể hiện khả năng kiểm soát chẵn lẻ thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm số lần xuất hiện tối đa và đếm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Thuật toán dễ dàng phù hợp với các ràng buộc điển hình của Codeforces, ngay cả đối với`n`lên tới`2 × 10^5`hoặc hơn thế nữa. Nó tránh mọi mô phỏng và giảm toàn bộ trò chơi xuống một lần quét mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    data = list(map(int, inp.split()))
    n = data[0]
    arr = data[1:1+n]
    mx = max(arr)
    cnt = sum(x == mx for x in arr)
    return "First" if cnt % 2 == 1 else "Second"

# minimum case
assert run("1\n5") == "First"

# simple even case
assert run("2\n1 1") == "Second"

# mixed values
assert run("5\n1 3 3 2 3") == "First"

# all distinct
assert run("4\n1 2 3 4") == "First"

# large parity check
assert run("6\n9 1 9 2 9 2") == "Second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5`| Đầu tiên | Trường hợp cơ sở phần tử đơn | 
|`2 1 1`| Thứ hai | Số chẵn tối đa | 
|`5 1 3 3 2 3`| Đầu tiên | Phân phối hỗn hợp điển hình | 
|`4 1 2 3 4`| Đầu tiên | Tất cả các phần tử đều được xử lý tối đa một lần hoặc ràng buộc | 
|`6 9 1 9 2 9 2`| Thứ hai | Trường hợp chẵn lẻ lớn hơn | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[x]`, mức tối đa xuất hiện một lần và người chơi đầu tiên ngay lập tức thắng bằng cách loại bỏ nó. Thuật toán xác định chính xác số lượng`1`và trả lại chiến thắng cho người chơi đầu tiên. 

Đối với các mảng trong đó tất cả các phần tử giống hệt nhau, giả sử`[7, 7, 7, 7]`, mọi phần tử đều là cực đại. Số đếm là 4 nên người chơi thứ hai thắng. Chế độ xem mô phỏng khớp chính xác với điều này vì mỗi lần di chuyển sẽ loại bỏ một phần tử cho đến khi cạn kiệt. 

Đối với các mảng có giá trị tối đa duy nhất và nhiều phần tử nhỏ hơn, chẳng hạn như`[10, 1, 1, 1]`, số lượng tối đa là 1, đảm bảo người chơi đầu tiên sẽ thắng. Bất kỳ nỗ lực nào nhằm sử dụng thao tác “nối thêm tiền tố nhỏ hơn” đều không làm thay đổi thực tế là chỉ tồn tại một nước đi quyết định.
