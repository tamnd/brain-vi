---
title: "CF 50A - Cọc Domino"
description: "Chúng ta được yêu cầu đặt càng nhiều quân domino tiêu chuẩn càng tốt trên một bảng hình chữ nhật có kích thước _M_ x _N_. Mỗi domino bao phủ chính xác hai ô vuông liền kề và các quân domino không thể chồng lên nhau hoặc mở rộng ra ngoài bảng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 50
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 47"
rating: 800
weight: 50
solve_time_s: 65
verified: true
draft: false
---
[CF 50A - Đóng cọc Domino](https://codeforces.com/problemset/problem/50/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đặt càng nhiều quân domino tiêu chuẩn càng tốt trên một bảng hình chữ nhật có kích thước _M_ x _N_. Mỗi domino bao phủ chính xác hai ô vuông liền kề và các quân domino không thể chồng lên nhau hoặc mở rộng ra ngoài bảng. Dữ liệu đầu vào cung cấp cho chúng ta hai số nguyên _M_ và _N_, biểu thị chiều cao và chiều rộng của bảng theo hình vuông. Đầu ra là một số nguyên duy nhất: số lượng quân domino tối đa có thể được đặt theo các quy tắc này. Domino có thể xoay được nên chúng ta có thể đặt chúng theo chiều dọc hoặc chiều ngang tùy theo điều kiện nào phù hợp. 

Các ràng buộc rất nhỏ: _M_ và _N_ nhiều nhất là 16. Điều này ngay lập tức loại bỏ mọi lo ngại về độ phức tạp tính toán quy mô lớn. Thậm chí tổng số ô vuông trên bảng nhiều nhất là 16 × 16 = 256, do đó, bất kỳ thuật toán nào xử lý mỗi ô vuông một vài lần là đủ nhanh. Chúng ta không cần bất cứ thứ gì cầu kỳ như lập trình động hoặc quay lui để đạt được hiệu quả, mặc dù việc nghĩ về nó về mặt hình học sẽ rất hữu ích. 

Một số trường hợp khó nhận thấy. Nếu bàn cờ chỉ có một hàng hoặc một cột thì số lượng quân domino tối đa chỉ đơn giản là số ô vuông chia cho hai, làm tròn xuống. Ví dụ: bàn cờ 1×5 chỉ có thể chứa 2 quân domino vì 5 chia cho 2 bằng 2 còn lại. Một trường hợp cạnh khác xảy ra khi tổng số ô vuông là số lẻ. Hình vuông cuối cùng sẽ không bao giờ bị quân domino che phủ nên số lớn nhất luôn là tổng diện tích chia cho 2, không tính phần dư. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi cách có thể để đặt quân domino lên bàn cờ. Chúng ta có thể thử đệ quy mọi vị trí và hướng, đánh dấu các ô vuông được sử dụng và đếm cấu hình tối đa. Về nguyên tắc thì điều này đúng, nhưng số lượng cấu hình tăng theo cấp số nhân theo diện tích bo mạch. Ngay cả với 256 hình vuông, số lượng tập hợp con của hình vuông là 2^256, một con số rất lớn về mặt thiên văn. Vì vậy, trong khi brute-force hoạt động vì nó khám phá tất cả các vị trí hợp lệ, nó lại thất bại do sự bùng nổ tổ hợp. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là mỗi domino luôn bao phủ chính xác hai ô vuông. Tổng số ô vuông là _M_ × _N_. Mỗi quân domino sử dụng hai ô vuông, vì vậy số lượng quân domino tối đa mà chúng ta có thể đặt chỉ đơn giản là tổng số ô vuông chia cho hai. Nếu tổng số là chẵn thì bảng có thể được che phủ hoàn toàn. Nếu nó là số lẻ, một hình vuông sẽ vẫn chưa được phát hiện. Thông tin chuyên sâu này biến vấn đề thành một phép tính số học đơn giản: chia diện tích sàn cho 2. Cách tiếp cận này tận dụng cấu trúc của các quân domino và tính đồng nhất của bàn cờ, thay vì khám phá các vị trí riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(M×N)) | O(M×N) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên _M_ và _N_ thể hiện kích thước bảng. 
2. Tính tổng số ô vuông trên bảng là _M_ × _N_. Điều này thể hiện tổng không gian có sẵn cho quân domino. 
3. Chia tổng số ô vuông cho 2 bằng phép chia số nguyên. Điều này giải thích cho thực tế là mỗi domino bao phủ chính xác hai ô vuông. Nếu tổng diện tích là số lẻ, phép chia số nguyên sẽ tự động bỏ phần dư, để lại một hình vuông không bị che, điều này đúng. 
4. In kết quả. 

Tại sao nó hoạt động: điều bất biến là mỗi domino luôn chiếm đúng hai ô vuông và không thể chồng lên nhau. Việc đóng gói tối đa các quân domino không bao giờ có thể vượt quá một nửa tổng số ô vuông, vì mỗi quân domino phải sử dụng hai ô. Bất kỳ sự sắp xếp nào bao phủ nhiều hơn số ô vuông (M×N/2) sàn sẽ vi phạm giới hạn phạm vi. Sự đơn giản của bàn cờ (hình chữ nhật, không có lỗ) đảm bảo rằng cách chia này mang lại số lượng quân domino tối đa mà không cần phải xem xét các vị trí cụ thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, n = map(int, input().split())
print((m * n) // 2)
```Giải pháp này trước tiên sẽ đọc các kích thước của bảng, nhân chúng để có được tổng số ô vuông, sau đó chia cho hai bằng phép chia số nguyên. Chúng tôi sử dụng phép chia số nguyên để tự động xử lý các kích thước bảng lẻ, đảm bảo kết quả là số nguyên. Điều này mạnh mẽ đối với tất cả các giá trị trong giới hạn đầu vào, bao gồm các bảng nhỏ nhất (1×1) và lớn nhất (16×16). 

## Ví dụ đã hoạt động 

Đối với bảng 2 × 4: 

| m | n | m*n | (m*n)//2 | 
| --- | --- | --- | --- | 
| 2 | 4 | 8 | 4 | 

Phép tính xác nhận chúng ta có thể đặt 4 quân domino. Điều này phù hợp với đầu ra mẫu. 

Đối với bảng 3×3: 

| m | n | m*n | (m*n)//2 | 
| --- | --- | --- | --- | 
| 3 | 3 | 9 | 4 | 

Phép tính cho thấy chúng ta có thể đặt 4 quân domino, để lại một ô vuông không che chắn. Điều này thể hiện việc xử lý chính xác các khu vực lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một phép nhân và chia duy nhất | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Với những hạn chế, giải pháp này chạy trong thời gian không đổi và sử dụng bộ nhớ tối thiểu, thấp hơn nhiều so với giới hạn cho phép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    m, n = map(int, input().split())
    return str((m * n) // 2)

# Provided sample
assert run("2 4\n") == "4", "sample 1"

# Custom cases
assert run("1 1\n") == "0", "single square board"
assert run("1 5\n") == "2", "single row with odd length"
assert run("16 16\n") == "128", "largest even board"
assert run("3 3\n") == "4", "odd area 3x3 board"
assert run("2 3\n") == "3", "small rectangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | Bàn cờ có kích thước tối thiểu, không thể đặt bất kỳ quân domino nào | 
| 1 5 | 2 | Một hàng có số ô vuông lẻ | 
| 16 16 | 128 | Bảng kích thước tối đa, diện tích chẵn | 
| 3 3 | 4 | Tổng số ô vuông lẻ, để lại một ô không che | 
| 2 3 | 3 | Hình chữ nhật nhỏ chung, tay cầm có kích thước hỗn hợp | 

## Vỏ cạnh 

Đối với bảng 1×1, thuật toán tính 1_1=1, sau đó 1//2=0. Kết quả đầu ra là chính xác vì một hình vuông không thể giữ được quân domino. Đối với bảng 1×5, 1_5=5 thì 5//2=2. Đầu ra chính xác để lại một hình vuông không được che chắn. Đối với bảng 3×3, thuật toán tính 9//2=4, để trống một ô vuông. Trong mọi trường hợp, phép chia số nguyên đảm bảo số lượng domino tối đa chính xác mà không bị đếm quá mức hoặc thiếu điều kiện cạnh.
