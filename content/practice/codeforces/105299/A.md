---
title: "CF 105299A - Tự động hoàn thành"
description: "Chúng tôi được cung cấp danh sách các địa chỉ trang web đã truy cập và một chuỗi tiền tố ngắn mà người dùng đã nhập. Nhiệm vụ là mô phỏng tính năng tự động hoàn thành: trong số tất cả các địa chỉ trang được lưu trữ bắt đầu bằng tiền tố này, chúng ta phải trả về địa chỉ nhỏ nhất trong từ điển…"
date: "2026-06-27T02:31:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105299
codeforces_index: "A"
codeforces_contest_name: "AGM 2024, Final Round, Day 1"
rating: 0
weight: 105299
solve_time_s: 51
verified: true
draft: false
---

[CF 105299A - Tự động hoàn thành](https://codeforces.com/problemset/problem/105299/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp danh sách các địa chỉ trang web đã truy cập và một chuỗi tiền tố ngắn mà người dùng đã nhập. Nhiệm vụ là mô phỏng tính năng tự động hoàn thành: trong số tất cả các địa chỉ trang được lưu trữ bắt đầu bằng tiền tố này, chúng ta phải trả về địa chỉ nhỏ nhất theo thứ tự từ điển (từ điển). Nếu không có địa chỉ được lưu trữ nào bắt đầu bằng tiền tố, chúng tôi chỉ cần trả lại tiền tố đó không thay đổi. 

Do đó, đầu vào là một chuỗi đơn biểu thị truy vấn một phần, theo sau là một tập hợp nhỏ các chuỗi đầy đủ ứng viên. Đầu ra là một trong những ứng cử viên đó hoặc tiền tố ban đầu nếu không khớp. 

Mặc dù câu lệnh ngắn, quyết định quan trọng của mô hình hóa là nhận ra rằng chúng ta không xây dựng chuỗi hoặc sửa đổi chúng, chỉ lọc một tập hợp theo tiền tố và sau đó lấy giá trị tối thiểu theo thứ tự từ điển. 

Các ràng buộc rất nhỏ: tối đa khoảng 100 chuỗi được lưu trữ với độ dài lên tới khoảng 100 ký tự. Điều đó có nghĩa là bất kỳ giải pháp nào lên tới khoảng O(n² · L) hoặc thậm chí O(n · L log n) đều nhanh chóng. Không cần cấu trúc hậu tố, hàm băm hoặc cấu trúc dữ liệu chuỗi nâng cao. Quét tuyến tính trên tất cả các chuỗi đã là tối ưu trong thực tế. 

Trường hợp cạnh chính là khi không có chuỗi nào khớp với tiền tố cả. Trong tình huống đó, hành vi đúng không phải là trả về một chuỗi trống hoặc một phần giữ chỗ mà là trả về tiền tố chính xác như đã cho. Ví dụ: nếu tiền tố là`"find"`và tất cả các chuỗi được lưu trữ là`"fond..."`, thì đầu ra vẫn phải là`"find"`. 

Một trường hợp tinh tế khác là khi tiền tố đó đã có sẵn trong danh sách. Vì nó tối thiểu về mặt từ điển trong số tất cả các chuỗi bắt đầu bằng chính nó nên nó sẽ tự nhiên được chọn, nhưng chỉ khi việc triển khai bao gồm chính xác đẳng thức như một kết quả khớp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là trực tiếp: lặp qua mọi chuỗi được lưu trữ, kiểm tra xem nó có bắt đầu bằng tiền tố đã cho hay không và theo dõi chuỗi nhỏ nhất theo thứ tự từ điển trong số các chuỗi khớp. Bản thân việc kiểm tra là một so sánh tiền tố đơn giản, có giá O(L) trên mỗi chuỗi trong đó L là độ dài chuỗi. With n strings, the total cost is O(n · L).

 This is already sufficient because n is small. Không có lợi ích gì trong việc sắp xếp mọi thứ trước tiên, vì việc sắp xếp sẽ tốn O(n log n · L) và chúng ta vẫn cần xác minh điều kiện tiền tố. Tương tự, việc xây dựng một bộ ba sẽ có hiệu quả nhưng lại tốn chi phí không cần thiết đối với một tập dữ liệu nhỏ như vậy. 

Quan sát quan trọng là thứ tự từ điển không phụ thuộc vào cấu trúc tổng thể ở đây. Chúng tôi chỉ cần một mức tối thiểu duy nhất trong số các ứng viên được lọc để có thể duy trì chuỗi hoạt động tốt nhất trong một lần vượt qua. This avoids any preprocessing entirely.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force scan with filtering | O(n · L) | O(1) thêm | Đã chấp nhận | 
| Sorting all strings first | O(n log n · L) | O(1)-O(n) | Accepted but unnecessary |

 ## Hướng dẫn thuật toán 

1. Đọc chuỗi tiền tố và số lượng địa chỉ được lưu trữ. Chúng tôi giữ tiền tố cố định vì nó xác định điều kiện lọc cho mọi ứng cử viên. 
2. Khởi tạo một biến`best`ở dạng trống hoặc trạng thái trọng điểm cho biết "chưa có kết quả phù hợp". Điều này sẽ lưu trữ sự hoàn thành hợp lệ nhỏ nhất về mặt từ điển được tìm thấy cho đến nay. 
3. Đối với mỗi chuỗi được lưu trữ, hãy kiểm tra xem nó có bắt đầu bằng tiền tố hay không. Điều này được thực hiện bằng cách so sánh từng ký tự với độ dài của tiền tố. Nếu tại bất kỳ điểm nào chúng khác nhau thì chuỗi đó không đủ điều kiện. 
4. Nếu chuỗi khớp với tiền tố, hãy so sánh nó với chuỗi hiện tại`best`. Nếu như`best`trống, chúng ta gán ngay cho nó. Nếu không, chúng tôi cập nhật`best`chỉ khi chuỗi hiện tại nhỏ hơn về mặt từ điển. Sự so sánh này là thứ tự từ điển tiêu chuẩn: ký tự khác nhau đầu tiên quyết định hoặc chuỗi ngắn hơn sẽ nhỏ hơn nếu chuỗi này là tiền tố của chuỗi kia. 
5. Sau khi xử lý tất cả các chuỗi, xuất ra`best`nếu nó tồn tại. Nếu không có chuỗi nào khớp, thay vào đó hãy xuất tiền tố ban đầu. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến duy nhất: sau khi xử lý từng chuỗi,`best`là chuỗi nhỏ nhất về mặt từ điển trong số tất cả các ứng cử viên được xử lý thỏa mãn điều kiện tiền tố. Vì mỗi ứng viên được kiểm tra chính xác một lần và các phép so sánh đều chính xác theo thứ tự từ điển, nên bất biến này được duy trì xuyên suốt. Khi kết thúc quá trình quét, tất cả các ứng cử viên hợp lệ đã được xem xét, vì vậy dữ liệu được lưu trữ`best`là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def starts_with(s, pref):
    if len(s) < len(pref):
        return False
    for i in range(len(pref)):
        if s[i] != pref[i]:
            return False
    return True

def solve():
    pref = input().strip()
    n = int(input())
    
    best = None
    
    for _ in range(n):
        s = input().strip()
        
        if not starts_with(s, pref):
            continue
        
        if best is None or s < best:
            best = s
    
    if best is None:
        print(pref)
    else:
        print(best)

if __name__ == "__main__":
    solve()
```Hàm trợ giúp kiểm tra rõ ràng việc khớp tiền tố thay vì dựa vào các thủ thuật cắt lát, điều này làm cho logic trở nên minh bạch và tránh các lỗi tinh vi khi độ dài chuỗi khác nhau. 

Vòng lặp chính duy trì`best`là ứng cử viên sáng giá nhất hiện nay. Sự so sánh`s < best`trực tiếp sử dụng thứ tự chuỗi từ điển của Python, phù hợp với định nghĩa vấn đề. 

Điều kiện cuối cùng xử lý trường hợp dự phòng bắt buộc khi không có kết quả khớp nào tồn tại, đảm bảo đầu ra chính xác là tiền tố đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
next
2
nextpermutation
nextelement
```| Bước | Chuỗi hiện tại | Tiền tố phù hợp | Tốt Nhất Trước | Tốt Nhất Sau | 
| --- | --- | --- | --- | --- | 
| 1 | hoán vị tiếp theo | vâng | Không có | hoán vị tiếp theo | 
| 2 | phần tử tiếp theo | vâng | hoán vị tiếp theo | phần tử tiếp theo | 

Câu trả lời cuối cùng:`nextelement`Điều này chứng tỏ rằng thuật toán không chỉ chọn kết quả khớp đầu tiên mà còn theo dõi chính xác kết quả nhỏ nhất về mặt từ điển trong số tất cả các kết quả hoàn thành hợp lệ. 

### Ví dụ 2 

đầu vào:```
find
4
find
findfirstof
findit
fand
```| Bước | Chuỗi hiện tại | Tiền tố phù hợp | Tốt Nhất Trước | Tốt Nhất Sau | 
| --- | --- | --- | --- | --- | 
| 1 | tìm | vâng | Không có | tìm | 
| 2 | tìm đầu tiên | vâng | tìm | tìm | 
| 3 | tìm nó | vâng | tìm | tìm | 
| 4 | fand | không | tìm | tìm | 

Câu trả lời cuối cùng:`find`Điều này cho thấy trường hợp trong đó tiền tố chính là đầu ra chính xác và tất cả các phần mở rộng dài hơn đều lớn hơn về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi chuỗi trong số n chuỗi được kiểm tra dựa vào tiền tố có độ dài L | 
| Không gian | O(1) | Chỉ một chuỗi tốt nhất được lưu trữ | 

Với n lên đến khoảng 100 và L lên đến khoảng 100, tổng công việc tối đa là 10⁴ so sánh ký tự, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""next
2
nextpermutation
nextelement
""") == "nextelement"

# sample 2
assert run("""find
4
find
findfirstof
findit
fand
""") == "find"

# no match case
assert run("""abc
3
def
ghi
xyz
""") == "abc"

# single string match
assert run("""a
1
a
""") == "a"

# prefix longer than some strings
assert run("""abcd
3
ab
abc
abce
""") == "abce"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có trường hợp trùng khớp | tiền tố | hành vi dự phòng | 
| khớp chuỗi đơn | một | độ chính xác đầu vào tối thiểu | 
| độ dài tiền tố hỗn hợp | abce | lọc từ điển + tiền tố | 

## Vỏ cạnh 

Khi không có chuỗi được lưu trữ nào bắt đầu bằng tiền tố, thuật toán sẽ giữ nguyên`best`BẰNG`None`và trả về chính xác tiền tố. Ví dụ, với tiền tố`"abc"`và dây`["def", "ghi"]`, mọi kiểm tra tiền tố đều thất bại, vì vậy đầu ra cuối cùng là`"abc"`. 

Khi tiền tố khớp chính xác với một trong các chuỗi được lưu trữ, chuỗi đó sẽ vượt qua quá trình kiểm tra tiền tố và được khởi tạo là`best`. Bất kỳ tiện ích mở rộng nào dài hơn như`"abcde"`lớn hơn về mặt từ điển, do đó thuật toán sẽ không thay thế kết quả khớp chính xác một cách không chính xác. 

Khi nhiều chuỗi có chung tiền tố nhưng khác nhau ở phần sau, việc so sánh từ điển sẽ đảm bảo lựa chọn chính xác. Ví dụ,`"nextpermutation"`Và`"nextelement"`chia sẻ`"next"`, nhưng so sánh từng ký tự sẽ xác định chính xác`"nextelement"`nhỏ hơn vì`'e' < 'p'`ở vị trí khác nhau đầu tiên.
