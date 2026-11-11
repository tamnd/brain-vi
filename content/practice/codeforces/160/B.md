---
title: "CF 160B - Vé Xấu"
description: "Chúng ta được cấp một tấm vé được biểu diễn dưới dạng một chuỗi gồm 2n chữ số, trong đó n nằm trong khoảng từ 1 đến 100. Nửa đầu của tấm vé chứa n chữ số đầu tiên và nửa sau chứa n chữ số còn lại. Chúng tôi được yêu cầu xác định xem tấm vé đó có “chắc chắn là không may mắn hay không”."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 160
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 111 (Div. 2)"
rating: 1100
weight: 160
solve_time_s: 89
verified: true
draft: false
---

[CF 160B - Vé xui xẻo](https://codeforces.com/problemset/problem/160/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một vé được biểu thị dưới dạng một chuỗi gồm 2_n_ chữ số, trong đó _n_ nằm trong khoảng từ 1 đến 100. Nửa đầu của vé chứa _n_ chữ số đầu tiên và nửa sau chứa _n_ chữ số còn lại. Chúng tôi được yêu cầu xác định xem tấm vé đó có “chắc chắn không may mắn” hay không. Một vé chắc chắn không may mắn nếu có thể ghép từng chữ số ở nửa đầu với một chữ số ở nửa sau sao cho mọi chữ số ở nửa đầu đều nhỏ hơn hoàn toàn so với chữ số ở nửa sau được ghép của nó hoặc mọi chữ số ở nửa đầu được ghép hoàn toàn lớn hơn chữ số ở nửa sau được ghép của nó. Mỗi chữ số phải được sử dụng chính xác một lần trong ghép nối. 

Đầu vào đảm bảo rằng tổng chiều dài của vé tối đa là 200 chữ số. Con số này nhỏ, có nghĩa là ngay cả những cách tiếp cận có độ phức tạp thời gian bậc hai cũng có thể hoạt động về mặt kỹ thuật, nhưng giải pháp tuyến tính sử dụng tính năng sắp xếp sẽ tự nhiên và hiệu quả hơn nhiều. 

Một điểm tinh tế là vé có thể có các chữ số lặp lại. Ví dụ, nửa đầu có thể là`11`và nửa sau`22`. Ở đây, một phép kiểm tra ngây thơ chỉ so sánh tổng hoặc sử dụng kết hợp tham lam mà không sắp xếp có thể thất bại. Một trường hợp cạnh khác là khi cả hai nửa giống hệt nhau, chẳng hạn như`1234`chia thành`12`Và`34`. Chúng ta cần xác minh sự bất đẳng thức nghiêm ngặt cho từng cặp tương ứng sau khi sắp xếp để tránh câu trả lời “CÓ” sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thực hiện tất cả các hoán vị có thể có của nửa sau để tìm ra một song ánh trong đó một trong hai điều kiện bất đẳng thức nghiêm ngặt được thỏa mãn. Đối với _n_ lên đến 100, số hoán vị là 100!, một con số lớn về mặt thiên văn và không khả thi. Lực lượng vũ phu là đúng về mặt khái niệm vì nó kiểm tra tất cả các cặp có thể có, nhưng nó thất bại do vụ nổ tổ hợp. 

Thông tin chi tiết quan trọng là thứ tự tương đối của các chữ số quan trọng hơn các vị trí cụ thể. Nếu chúng ta sắp xếp cả hai nửa theo thứ tự tăng dần, chúng ta có thể so sánh các chữ số theo cặp: hoặc các chữ số được sắp xếp ở nửa đầu đều nhỏ hơn hoàn toàn so với các chữ số được sắp xếp ở nửa sau hoặc tất cả đều lớn hơn. Việc sắp xếp đảm bảo rằng mỗi chữ số nhỏ nhất được khớp với chữ số nhỏ nhất của nửa còn lại, điều này cần và đủ để kiểm tra tiêu chí kém may mắn. Điều này làm giảm vấn đề xuống còn quét tuyến tính đơn giản sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Sắp xếp + Kiểm tra theo cặp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ và chuỗi vé có độ dài 2_n_. Chia sợi dây thành hai nửa,`first_half`Và`second_half`. Sự tách biệt này cho phép chúng ta suy luận về hai nhóm một cách độc lập. 
2. Chuyển mỗi nửa thành một danh sách các chữ số nguyên. Điều này làm cho việc so sánh trở nên đơn giản và tránh phải xử lý các mã ký tự chuỗi. 
3. Sắp xếp cả hai nửa theo thứ tự tăng dần. Sắp xếp là bước cốt lõi giúp giảm bớt vấn đề từ việc xem xét tất cả các hoán vị thành một cặp chính tắc duy nhất. 
4. So sánh các chữ số được sắp xếp theo cặp. Khởi tạo hai cờ:`all_first_less`Và`all_first_greater`. Đối với mỗi chỉ số`i`từ 0 đến n-1, nếu chữ số từ`first_half`không nhỏ hơn tương ứng`second_half`chữ số, đặt`all_first_less`thành sai. Tương tự, nếu chữ số từ`first_half`không lớn hơn tương ứng`second_half`chữ số, đặt`all_first_greater`thành sai. 
5. Sau khi quét, nếu một trong hai`all_first_less`hoặc`all_first_greater`vẫn đúng, in`YES`. Ngược lại, in`NO`. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo rằng mỗi chữ số ở nửa đầu được ghép với chữ số ở nửa sau nhỏ nhất còn lại. Nếu bất kỳ bất đẳng thức nghiêm ngặt nào không thành công sau khi sắp xếp thì không có hoán vị nào khác có thể thỏa mãn điều kiện. Vì vậy, việc so sánh có sắp xếp là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
ticket = input().strip()

first_half = sorted(int(x) for x in ticket[:n])
second_half = sorted(int(x) for x in ticket[n:])

all_first_less = True
all_first_greater = True

for a, b in zip(first_half, second_half):
    if a >= b:
        all_first_less = False
    if a <= b:
        all_first_greater = False

print("YES" if all_first_less or all_first_greater else "NO")
```Giải pháp đầu tiên đọc đầu vào và tách vé thành hai nửa. Sắp xếp đảm bảo chúng ta có thể so sánh trực tiếp các chữ số tương ứng. Vòng lặp kiểm tra đồng thời các điều kiện bất đẳng thức nghiêm ngặt. sử dụng`zip`là một cách rõ ràng và ngắn gọn để ghép các chữ số, tránh các lỗi sai sót một. Điều kiện cuối cùng in kết quả chính xác dựa trên các cờ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
2421
```| đầu_nửa | giây_nửa | a >= b | một <= b | all_first_less | all_first_Greater | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | Đúng | Sai | Sai | Đúng | 
| 4 | 2 | Đúng | Sai | Sai | Đúng | 

Đầu ra của thuật toán`YES`bởi vì`all_first_greater`vẫn Đúng, xác nhận vé đáp ứng tiêu chí kém may mắn. 

Đầu vào mẫu 2:```
2
1234
```| đầu_nửa | giây_nửa | a >= b | một <= b | all_first_less | all_first_Greater | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | Sai | Đúng | Đúng | Sai | 
| 2 | 4 | Sai | Đúng | Đúng | Sai | 

Đầu ra của thuật toán`YES`bởi vì`all_first_less`vẫn đúng. Việc sắp xếp đảm bảo rằng mỗi chữ số nửa đầu nhỏ nhất được so sánh với chữ số nửa thứ hai nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cả hai nửa chiếm ưu thế; quét so sánh là tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách chữ số | 

Với n lên đến 100, việc sắp xếp và quét chỉ diễn ra trong vòng 2 giây. Việc sử dụng bộ nhớ cũng ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    ticket = input().strip()
    first_half = sorted(int(x) for x in ticket[:n])
    second_half = sorted(int(x) for x in ticket[n:])
    all_first_less = True
    all_first_greater = True
    for a, b in zip(first_half, second_half):
        if a >= b:
            all_first_less = False
        if a <= b:
            all_first_greater = False
    return "YES" if all_first_less or all_first_greater else "NO"

# Provided sample
assert run("2\n2421\n") == "YES", "sample 1"

# Custom cases
assert run("2\n1234\n") == "YES", "increasing halves"
assert run("2\n1221\n") == "NO", "mixed digits"
assert run("1\n12\n") == "YES", "minimum size, first less"
assert run("1\n21\n") == "YES", "minimum size, first greater"
assert run("3\n111222\n") == "YES", "all equal in halves, first less"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1234 | CÓ | Điều kiện ít nghiêm ngặt theo cặp sau khi sắp xếp | 
| 2\n1221 | KHÔNG | Các chữ số hỗn hợp không thể thỏa mãn bất đẳng thức nghiêm ngặt | 
| 1\n12 | CÓ | Tối thiểu n=1, đầu tiên ít hơn | 
| 1\n21 | CÓ | Tối thiểu n=1, lớn hơn lần đầu | 
| 3\n111222 | CÓ | Nhiều chữ số bằng nhau, so sánh được sắp xếp | 

## Vỏ cạnh 

Để có được một tấm vé như`1221`(nửa đầu`12`, nửa sau`21`), việc sắp xếp tạo ra`12`Và`12`. So sánh theo cặp, chữ số đầu tiên 1 so với 1 vi phạm sự bất bình đẳng nghiêm ngặt cho cả số nhỏ hơn và số lớn hơn, do đó cả hai cờ đều trở thành sai. Thuật toán xuất ra chính xác`NO`. 

Đối với vé có kích thước tối thiểu, chẳng hạn như`12`, sự so sánh là ngay lập tức: nửa đầu 1 < nửa sau 2, vì vậy`all_first_less`vẫn đúng, mang lại`YES`. 

Đối với vé có chữ số lặp lại trên một nửa, chẳng hạn như`111222`, việc sắp xếp đảm bảo rằng các chữ số nhỏ nhất được ghép nối. Hiệp một`111`, hiệp hai`222`, tất cả các chữ số ở nửa đầu hoàn toàn nhỏ hơn, tạo ra`YES`. Thuật toán xử lý các bản sao một cách chính xác.
