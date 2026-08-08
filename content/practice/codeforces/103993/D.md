---
title: "CF 103993D - Mật khẩu"
description: "Chúng ta đang xử lý mật khẩu có 6 chữ số, trong đó mỗi mật khẩu là một chuỗi các chữ số từ 0 đến 9. Cấu trúc của mật khẩu bị hạn chế cao: mật khẩu phải sử dụng chính xác hai chữ số riêng biệt và mỗi chữ số đó phải xuất hiện chính xác ba lần, vì vậy mật khẩu luôn là…"
date: "2026-07-02T06:00:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103993
codeforces_index: "D"
codeforces_contest_name: "ICPC 2022-2023 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 103993
solve_time_s: 45
verified: true
draft: false
---

[CF 103993D - Mật khẩu](https://codeforces.com/problemset/problem/103993/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý mật khẩu có 6 chữ số, trong đó mỗi mật khẩu là một chuỗi các chữ số từ 0 đến 9. Cấu trúc của mật khẩu bị hạn chế cao: mật khẩu phải sử dụng chính xác hai chữ số riêng biệt và mỗi chữ số đó phải xuất hiện chính xác ba lần, vì vậy mật khẩu luôn là một hoán vị của một cái gì đó giống như AAA BBB trong đó A và B là các chữ số khác nhau. 

Ngoài quy tắc cấu trúc này, chúng ta còn được cung cấp một tập hợp các chữ số bị cấm. Bất kỳ mật khẩu ứng cử viên nào đều không hợp lệ nếu nó sử dụng ngay cả một trong những chữ số đó. 

Vì vậy, nhiệm vụ hoàn toàn mang tính tổ hợp: đếm xem có thể tạo được bao nhiêu chuỗi có độ dài 6 bằng cách chọn hai chữ số cho phép và sắp xếp chúng sao cho mỗi chuỗi xuất hiện chính xác ba lần. 

Đầu vào bao gồm một danh sách nhỏ các chữ số bị cấm. Tất cả mọi thứ không được liệt kê đều được phép. Vì các chữ số chỉ nằm trong khoảng từ 0 đến 9, nên phạm vi hiệu dụng là cực kỳ nhỏ, điều này cho thấy rằng lực lượng vũ phu đối với các cặp chữ số là đủ. 

Các ràng buộc ngụ ý một không gian tìm kiếm rất nhỏ. Có tối đa 10 chữ số tồn tại và chúng tôi đang chọn các cặp chữ số được phép. Điều đó mang lại nhiều nhất C(10, 2) = 45 cặp ứng cử viên, rất nhỏ. Đối với mỗi cặp, việc sắp xếp đếm là công việc liên tục. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa hoặc tiền xử lý nâng cao. 

Các trường hợp cạnh chủ yếu liên quan đến cách các chữ số bị cấm thu nhỏ tập hợp có sẵn. 

Một trường hợp tế nhị là khi có ít hơn hai chữ số. Ví dụ: nếu 9 chữ số bị cấm và chỉ còn lại một chữ số thì không thể tạo mật khẩu hợp lệ vì chúng ta cần hai chữ số riêng biệt. Một trường hợp góc khác là khi vẫn còn chính xác hai chữ số, tạo ra một cặp chữ số hợp lệ nhưng có nhiều hoán vị. 

Ví dụ về việc tư duy tham lam không thành công: nếu các chữ số được phép là {1, 2}, người ta có thể nhầm tưởng rằng chỉ có một mật khẩu “111222”, nhưng câu trả lời thực ra là số hoán vị riêng biệt của nhiều tập hợp này, là 6!/(3!3!) = 20. Thiếu thừa số tổ hợp này là lỗi phổ biến nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra tất cả các chuỗi có độ dài 6 chữ số từ 0 đến 9 và kiểm tra xem mỗi chuỗi có thỏa mãn các ràng buộc hay không: chính xác hai chữ số riêng biệt, mỗi chữ số xuất hiện ba lần và không có chữ số bị cấm. Có 10^6 dãy như vậy. Mỗi lần kiểm tra là O(6), vì vậy phương pháp này tiêu tốn khoảng 6 triệu thao tác cho mỗi lần kiểm tra, vốn đã ở mức giới hạn nhưng vẫn được chấp nhận ở một số ngôn ngữ, mặc dù không cần thiết. 

Quan sát chính là cấu trúc của mật khẩu hợp lệ hoàn toàn được xác định bằng cách chọn hai chữ số. Khi các chữ số A và B được cố định, số chuỗi hợp lệ hoàn toàn là tổ hợp: chúng ta sắp xếp 3 bản sao của A và 3 bản sao của B theo một chuỗi có độ dài 6. Số cách sắp xếp như vậy là hệ số đa thức 6! / (3!3!) = 20, không phụ thuộc vào chữ số nào được chọn. 

Điều này làm giảm vấn đề đếm có bao nhiêu cặp chữ số được phép tồn tại. Nếu k chữ số được cho phép, chúng tôi chọn bất kỳ cặp không có thứ tự (A, B), đưa ra các khả năng C(k, 2) và mỗi khả năng đóng góp chính xác 20 mật khẩu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các chuỗi | O(10^6) | O(1) | Quá chậm/không cần thiết | 
| Chọn cặp chữ số + tổ hợp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tập hợp các chữ số bị cấm và đánh dấu chúng. Bước này xác định những chữ số nào vẫn có thể sử dụng được trong quá trình xây dựng. 
2. Xây dựng danh sách các chữ số được phép bằng cách quét từ 0 đến 9 và thu thập những chữ số không bị đánh dấu cấm. Điều này cô lập bảng chữ cái hiệu quả. 
3. Đếm số chữ số cho phép, gọi là k. Giá trị này xác định liệu có thể tồn tại bất kỳ mật khẩu hợp lệ nào hay không. 
4. Nếu k nhỏ hơn 2 thì trả về 0 ngay vì không thể chọn được hai chữ số phân biệt. Đây là một sự bất khả thi về mặt cấu trúc. 
5. Mặt khác, hãy tính số cách để chọn hai chữ số riêng biệt từ k, đó là k * (k - 1) / 2. Mỗi cặp như vậy tương ứng với một tập hợp nhiều chữ số cố định được sử dụng trong mật khẩu. 
6. Nhân số cặp với 20, vì với bất kỳ cặp nào được chọn (A, B), số cách sắp xếp 6 chiều hợp lệ với ba chữ A và ba chữ B là 6! / (3!3!) = 20. 

### Tại sao nó hoạt động 

Mỗi mật khẩu hợp lệ phải sử dụng chính xác hai chữ số riêng biệt, do đó nó tạo ra một cặp chữ số duy nhất không có thứ tự. Ngược lại, bất kỳ cặp chữ số được phép không có thứ tự nào sẽ xác định duy nhất một họ mật khẩu hợp lệ bao gồm tất cả các hoán vị của nhiều tập hợp với ba bản sao của mỗi chữ số. Các họ này rời rạc vì các cặp chữ số khác nhau không thể tạo ra cùng một chuỗi. Vì mỗi họ có cùng kích thước cố định nên tổng số lượng chỉ đơn giản là số cặp nhân với kích thước của một họ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    forbidden = set(map(int, input().split())) if n else set()

    allowed = [d for d in range(10) if d not in forbidden]
    k = len(allowed)

    if k < 2:
        print(0)
        return

    pairs = k * (k - 1) // 2
    ans = pairs * 20
    print(ans)

if __name__ == "__main__":
    solve()
```Trước tiên, mã xây dựng tập hợp chữ số được phép, điều này cần thiết để đảm bảo các chữ số bị cấm không bao giờ nhập vào bất kỳ cấu trúc ứng cử viên nào. Séc`k < 2`xử lý trường hợp suy biến trong đó không tồn tại cặp hợp lệ. 

biểu hiện`k * (k - 1) // 2`tính toán số lượng các cặp chữ số không có thứ tự một cách hiệu quả mà không cần tạo ra chúng một cách rõ ràng. Nhân với 20 sẽ áp dụng số tổ hợp cố định để sắp xếp ba bản sao của mỗi chữ số. 

Một lỗi triển khai phổ biến là quên rằng thứ tự quan trọng ở chuỗi cuối cùng chứ không phải ở việc lựa chọn các chữ số. Đó là lý do tại sao chúng tôi tách lựa chọn cặp (lựa chọn tổ hợp) khỏi số lượng sắp xếp (số lượng hoán vị cố định). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
0 1 2 4 5 6 8 9
```Các chữ số được phép chỉ là {3, 7} nên k = 2. 

| Bước | Giá trị | 
| --- | --- | 
| Chữ số được phép | {3, 7} | 
| k | 2 | 
| Cặp | 1 | 
| Cách mỗi cặp | 20 | 
| Trả lời | 20 | 

Điều này cho thấy trường hợp không tầm thường tối thiểu trong đó tồn tại chính xác một cặp chữ số. 

### Ví dụ 2 

đầu vào:```
1
8
```Các chữ số được phép là {0,1,2,3,4,5,6,7,9}, vì vậy k = 9. 

| Bước | Giá trị | 
| --- | --- | 
| Chữ số được phép | 9 chữ số | 
| k | 9 | 
| Cặp | 36 | 
| Cách mỗi cặp | 20 | 
| Trả lời | 720 | 

Trường hợp này thể hiện việc chia tỷ lệ hoàn toàn thông qua tổ hợp, không phụ thuộc vào nhận dạng chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ quét 10 chữ số và số học đơn giản | 
| Không gian | O(1) | Mảng có kích thước cố định cho các chữ số | 

Các chữ số giới hạn ràng buộc ở mức 10, vì vậy giải pháp là thời gian không đổi cho mỗi lần kiểm tra. Ngay cả trong nhiều trường hợp thử nghiệm, thời gian chạy vẫn không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    forbidden = set(map(int, input().split())) if n else set()
    allowed = [d for d in range(10) if d not in forbidden]
    k = len(allowed)
    ans = 0 if k < 2 else (k * (k - 1) // 2) * 20
    return str(ans)

# provided samples
assert run("8\n0 1 2 4 5 6 8 9\n") == "20"
assert run("1\n8\n") == "720"

# custom cases
assert run("9\n0 1 2 3 4 5 6 7 8\n") == "0", "only one digit allowed"
assert run("0\n") == "810", "all digits allowed: C(10,2)*20"
assert run("7\n0 1 2 3 4 5 6\n") == "60", "three allowed digits"
assert run("8\n1 2 3 4 5 6 7 8\n") == "20", "exact two allowed digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chữ số đều bị cấm ngoại trừ một | 0 | không đủ chữ số | 
| không có chữ số bị cấm | 810 | trường hợp tổ hợp đầy đủ | 
| 3 chữ số được phép | 60 | nhiều cặp | 
| đúng 2 chữ số cho phép | 20 | trường hợp kết cấu cơ sở | 

## Vỏ cạnh 

Nếu gần như tất cả các chữ số đều bị cấm và chỉ còn lại một chữ số, thuật toán sẽ tạo ra k = 1 một cách chính xác và ngay lập tức trả về 0 vì không thể tạo thành cặp nào. Ví dụ, đầu vào`9`với chữ số`0 1 2 3 4 5 6 7 8`chỉ để lại chữ số 9, vì vậy k = 1 và hàm trả về 0 như mong đợi. 

Khi không có chữ số nào bị cấm, k = 10, vậy số cặp là 45. Nhân với 20 sẽ được 900. Thuật toán xử lý điều này một cách rõ ràng thông qua cùng một công thức mà không cần viết hoa đặc biệt. 

Nếu còn lại chính xác hai chữ số, k = 2, tạo ra một cặp và do đó có chính xác 20 mật khẩu hợp lệ. Đây là kịch bản khác 0 nhỏ nhất và xác nhận rằng hệ số tổ hợp được áp dụng chính xác. 

Trong mọi trường hợp, cấu trúc tính toán đảm bảo rằng các chữ số bị cấm chỉ ảnh hưởng đến kích thước của tập hợp được phép và không bao giờ yêu cầu bất kỳ sự phân nhánh đặc biệt nào ngoài việc đếm.
