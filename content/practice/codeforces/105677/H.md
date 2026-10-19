---
title: "CF 105677H - Vua của SWERC"
description: "Chúng ta được cung cấp một chuỗi tên đại diện cho phiếu bầu trong một cuộc bầu cử. Mỗi dòng tương ứng với một phiếu bầu cho một ứng cử viên và mỗi ứng cử viên được xác định bằng một chuỗi chữ hoa. Nhiệm vụ là xác định ứng cử viên nào nhận được nhiều phiếu bầu hơn mọi ứng cử viên khác."
date: "2026-06-22T05:07:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 43
verified: true
draft: false
---

[CF 105677H - Vua của SWERC](https://codeforces.com/problemset/problem/105677/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tên đại diện cho phiếu bầu trong một cuộc bầu cử. Mỗi dòng tương ứng với một phiếu bầu cho một ứng cử viên và mỗi ứng cử viên được xác định bằng một chuỗi chữ hoa. Nhiệm vụ là xác định ứng cử viên nào nhận được nhiều phiếu bầu hơn mọi ứng cử viên khác. 

Chi tiết cấu trúc quan trọng là đầu vào đảm bảo người chiến thắng duy nhất. Điều này loại bỏ sự cần thiết phải lý luận về các ràng buộc hoặc các quy tắc phụ. Chúng ta chỉ cần xác định tên có tần suất cao nhất. 

Các ràng buộc rất nhỏ, tối đa là 500 phiếu bầu và mỗi tên có độ dài tối đa là 20. Điều này ngay lập tức cho thấy rằng ngay cả các chiến lược đếm đơn giản cũng sẽ hoạt động thoải mái. Một giải pháp tuyến tính về số phiếu bầu, hoặc thậm chí là bậc hai trong trường hợp xấu nhất, vẫn có thể được chấp nhận. Tuy nhiên, vì kích thước đầu vào rất nhỏ nên cách tiếp cận rõ ràng nhất là sử dụng bản đồ tần số. 

Một trường hợp khó phát hiện thường gây khó khăn cho việc triển khai đơn giản là quên rằng các tên lặp lại theo thứ tự tùy ý và có thể không được nhóm lại. Ví dụ: một đầu vào như 

JON 

JOFFREY 

TYWIN 

JON 

yêu cầu tổng hợp số lượng trên các vị trí không liền kề. Bất kỳ cách tiếp cận nào chỉ kiểm tra các bản sao liên tiếp sẽ thất bại ở đây. Một cạm bẫy tiềm tàng khác là cho rằng thứ tự từ điển đóng bất kỳ vai trò nào nhưng nó không liên quan vì tần suất bỏ phiếu là yếu tố quyết định duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi tên là một ứng cử viên và quét toàn bộ danh sách để đếm xem nó xuất hiện bao nhiêu lần. Đối với mỗi phiếu bầu, chúng tôi tính toán lại tổng tần suất của nó bằng cách lặp lại tất cả các phiếu bầu. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của “thường xuyên nhất”, nhưng nó thực hiện công việc dư thừa. Nếu có N phiếu bầu, chúng tôi thực hiện công việc đếm O(N) cho mỗi mục trong số N mục, dẫn đến hoạt động O(N²). 

Với N 500, giá trị này vẫn nhỏ về mặt số lượng nhưng không hiệu quả về mặt cấu trúc và không cần thiết. Quan sát quan trọng là việc đếm tần số không cần quét lặp lại. Chúng ta chỉ cần tổng hợp số lượng một lần. Bản đồ băm hoặc từ điển cho phép chúng tôi tích lũy tần số trong một lần chuyển, cập nhật số lượng khi chúng tôi đọc mỗi phiếu bầu. Sau đó, chúng ta chỉ cần tìm mức tối đa. 

Điều này làm giảm công việc lặp đi lặp lại thành một lần duyệt tuyến tính duy nhất, sau đó là một lần quét tuyến tính khác trên các tên riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Được chấp nhận nhưng dư thừa | 
| Bản đồ tần số | O(N) | O(K) | Đã chấp nhận | 

Ở đây K là số tên riêng biệt. 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống để lưu trữ số phiếu bầu được khóa theo tên ứng cử viên. Cấu trúc này thể hiện kiến ​​thức tích lũy được về cuộc bầu cử khi chúng ta xử lý phiếu bầu một cách tuần tự. 
2. Đọc từng phiếu bầu một. Với mỗi tên, hãy tăng số lượng của nó trong từ điển. Nếu tên chưa xuất hiện, hãy khởi tạo số đếm của nó thành 1. Bước này đảm bảo rằng mỗi phiếu bầu được tính chính xác một lần. 
3. Duy trì các biến số để theo dõi ứng cử viên tốt nhất hiện tại và số phiếu bầu của họ. Khi cập nhật số lượng, chúng tôi có thể tính toán lại mức tối đa ở cuối hoặc cập nhật tăng dần. Cách đơn giản nhất và ít xảy ra lỗi nhất là tính toán nó sau khi xây dựng bảng tần số. 
4. Sau khi xử lý tất cả các phiếu bầu, lặp lại các mục từ điển và chọn tên có tần suất tối đa. Bởi vì vấn đề đảm bảo một người chiến thắng duy nhất nên chúng ta không cần logic ràng buộc. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào trong quá trình xử lý, từ điển sẽ lưu trữ số lần xuất hiện chính xác của từng tên được nhìn thấy cho đến nay. Vì mỗi phiếu bầu được xử lý chính xác một lần và chỉ tăng một bộ đếm duy nhất nên không có thông tin nào bị mất hoặc được tính hai lần. Sau khi tất cả các phiếu bầu được xử lý, bảng tần số là sự thể hiện chính xác của tập dữ liệu. Do đó, việc chọn giá trị tối đa trên bảng này tương đương với việc chọn tên thường xuyên nhất trong đầu vào ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    
    freq = {}
    
    for _ in range(n):
        name = input().strip()
        freq[name] = freq.get(name, 0) + 1
    
    best_name = ""
    best_count = -1
    
    for name, cnt in freq.items():
        if cnt > best_count:
            best_count = cnt
            best_name = name
    
    print(best_name)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng từ điển được khóa bằng chuỗi để tích lũy số lượng. Mỗi dòng đầu vào được lược bỏ để loại bỏ các ký tự dòng mới. các`.get()`phương pháp tránh kiểm tra sự tồn tại rõ ràng. Sau khi đọc tất cả các phiếu bầu, chúng tôi thực hiện quét lần cuối các mục từ điển để tìm mức tối đa. 

Một chi tiết triển khai tinh tế đang được khởi tạo`best_count`thành -1, đảm bảo rằng ngay cả một phiếu bầu cũng cập nhật chính xác câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
RAMSES
```| Bước | Bình chọn | Bản đồ tần số | Ứng viên xuất sắc nhất | 
| --- | --- | --- | --- | 
| 1 | RAM | {RAMSES: 1} | RAM | 

Cuộc bỏ phiếu duy nhất ngay lập tức xác định người chiến thắng vì không có ứng cử viên cạnh tranh. 

Điều này xác nhận rằng thuật toán xử lý chính xác kích thước đầu vào tối thiểu mà không yêu cầu cách viết hoa đặc biệt. 

### Ví dụ 2 

đầu vào:```
4
JON
JOFFREY
TYWIN
JON
```| Bước | Bình chọn | Bản đồ tần số | Ứng viên xuất sắc nhất | 
| --- | --- | --- | --- | 
| 1 | JON | {JON: 1} | JON | 
| 2 | JOFFREY | {JON: 1, JOFFREY: 1} | JON (hòa, nhìn thấy lần đầu) | 
| 3 | TYWIN | {JON: 1, JOFFREY: 1, TYWIN: 1} | JON | 
| 4 | JON | {JON: 2, JOFFREY: 1, TYWIN: 1} | JON | 

Sau tất cả các bản cập nhật, JON có tần suất cao nhất. 

Ví dụ này cho thấy rằng các phiếu bầu không liên tiếp lặp lại được tổng hợp chính xác và các mối quan hệ trong các bước trung gian không ảnh hưởng đến tính chính xác do lựa chọn cuối cùng diễn ra sau khi xử lý đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lần để xây dựng tần số và một lần vượt qua các tên riêng biệt | 
| Không gian | O(K) | Lưu trữ số lượng K tên duy nhất | 

Các ràng buộc cho phép tối đa 500 phiếu bầu, do đó, ngay cả chi phí vận hành từ điển cũng không đáng kể. Giải pháp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    
    input = _sys.stdin.readline
    
    n = int(input().strip())
    freq = {}
    
    for _ in range(n):
        name = input().strip()
        freq[name] = freq.get(name, 0) + 1
    
    best_name = ""
    best_count = -1
    
    for name, cnt in freq.items():
        if cnt > best_count:
            best_count = cnt
            best_name = name
    
    return best_name + "\n"

# provided samples
assert run("1\nRAMSES\n") == "RAMSES\n"
assert run("4\nJON\nJOFFREY\nTYWIN\nJON\n") == "JON\n"

# custom cases
assert run("3\nA\nB\nA\n") == "A\n"
assert run("5\nZ\nZ\nZ\nY\nX\n") == "Z\n"
assert run("2\nALICE\nBOB\nALICE\n") == "ALICE\n"
assert run("1\nKING\n") == "KING\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 VUA | VUA | Đầu vào kích thước tối thiểu | 
| A B A | A | Xử lý lặp lại cơ bản | 
| Z Z Z Y X | Z | Trường hợp tần số chiếm ưu thế | 
| ALICE BOB ALICE | ALICE | Bản sao không liền kề | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi người chiến thắng xuất hiện ở các vị trí riêng biệt thay vì được nhóm lại. Ví dụ, ở đầu vào```
3
A
B
A
```từ điển phát triển như`{A: 1}`,`{A: 1, B: 1}`,`{A: 2, B: 1}`. Mức tối đa cuối cùng xác định chính xác A, mặc dù nó không liền kề. Bất kỳ cách tiếp cận nào dựa vào sự liền kề sẽ coi đây là hai phân đoạn A không liên quan một cách không chính xác. 

Một trường hợp cạnh khác là đầu vào nhỏ nhất có thể:```
1
KING
```Bảng tần số trở thành`{KING: 1}`ngay lập tức. Bước lựa chọn tối đa chỉ đơn giản trả về KING, cho thấy rằng không cần logic đặc biệt cho các trường hợp suy biến. 

Cuối cùng, trường hợp tất cả các tên đều giống hệt nhau, chẳng hạn như```
4
A
A
A
A
```tạo ra một mục từ điển duy nhất`{A: 4}`. Thuật toán xử lý việc này một cách tự nhiên mà không có sự mơ hồ và mức tối đa vẫn nhất quán trong tất cả các bước.
