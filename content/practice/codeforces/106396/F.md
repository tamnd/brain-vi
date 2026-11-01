---
title: "CF 106396F - \u6597\u95ef\u5c06"
description: "Nhiệm vụ về cơ bản là so sánh trực tiếp giữa hai số nguyên. Mỗi trường hợp thử nghiệm cung cấp hai số và đầu ra chỉ phụ thuộc vào thứ tự tương đối của chúng. Nếu cả hai giá trị đều giống nhau thì kết quả là hòa."
date: "2026-06-19T18:06:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "F"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 39
verified: true
draft: false
---

[CF 106396F - \u6597\u95ef\u5c06](https://codeforces.com/problemset/problem/106396/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ về cơ bản là so sánh trực tiếp giữa hai số nguyên. Mỗi trường hợp thử nghiệm cung cấp hai số và đầu ra chỉ phụ thuộc vào thứ tự tương đối của chúng. Nếu cả hai giá trị đều giống nhau thì kết quả là hòa. Nếu giá trị đầu tiên lớn hơn, một người tham gia sẽ thắng; nếu không thì người tham gia khác sẽ thắng. 

Từ góc độ thuật toán, kích thước đầu vào không liên quan vì không có cấu trúc nào để xử lý ngoài việc đọc và so sánh hai số. Ngay cả khi bài toán có nhiều ca kiểm thử, mỗi ca kiểm thử vẫn độc lập và chỉ yêu cầu thời gian làm việc không đổi. 

Vì mỗi thao tác là một quyết định so sánh và đầu ra đơn giản nên ngay cả đầu vào cực lớn cũng không đe dọa đến hiệu suất. Bất kỳ giải pháp nào tránh được chi phí phân tích cú pháp không cần thiết hoặc cấu trúc dữ liệu bổ sung sẽ chạy thoải mái trong giới hạn. 

Trường hợp cạnh tinh tế duy nhất là sự bình đẳng. Việc triển khai đơn giản chỉ kiểm tra lớn hơn và nhỏ hơn mà không xử lý rõ ràng đẳng thức có thể vô tình rơi vào các nhánh logic và tạo ra kết quả đầu ra hoặc hành vi mặc định không chính xác. Ví dụ: nếu cả hai số bằng nhau và chương trình chỉ kiểm tra`a > b`và nếu không thì in ra một kẻ thua cố định, nó sẽ chỉ định sai một người chiến thắng thay vì tuyên bố hòa. 

Ví dụ về hành vi đúng: 

đầu vào:`5 5`Đầu ra:`Pingju`Một chuỗi điều kiện bất cẩn có thể coi đây là trường hợp “khác” của`a > b`, in sai phần thắng thay vì công nhận sự bình đẳng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực vẫn là so sánh trực tiếp hai con số. Không có cách giải thích thay thế có ý nghĩa nào cho vấn đề gây ra sự phức tạp như tìm kiếm, sắp xếp hoặc duyệt đồ thị. Mọi nỗ lực mô phỏng hoặc biến đổi đầu vào sẽ chỉ làm tăng thêm chi phí không cần thiết. 

Quan sát quan trọng là bài toán quy giản hoàn toàn thành phép so sánh ba chiều giữa hai số nguyên. Khi điều này được nhận ra, giải pháp sẽ trở thành một kiểm tra có điều kiện duy nhất. Không cần tiền xử lý hoặc cấu trúc dữ liệu phụ trợ. 

Giải pháp tối ưu chỉ đơn giản là phân nhánh theo đẳng thức trước, sau đó xác định giá trị nào lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Logic so sánh trực tiếp | O(1) | O(1) | Đã chấp nhận | 
| Bất kỳ mô phỏng mở rộng nào | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên`a`Và`b`từ đầu vào. Chúng đại diện cho điểm số hoặc giá trị của hai đối thủ cạnh tranh. 
2. Kiểm tra xem`a`bằng với`b`. Nếu chúng khớp chính xác, hãy xuất`"Pingju"`, vì không bên nào có lợi thế. 
3. Nếu không bằng nhau thì so sánh trực tiếp. Nếu như`a > b`, đầu ra`"Liang"`. 
4. Nếu không,`a < b`, do đó đầu ra`"Chuang"`. 

Cấu trúc của quyết định đảm bảo rằng sự bình đẳng được xử lý trước khi ra lệnh so sánh, ngăn ngừa sự mơ hồ. 

### Tại sao nó hoạt động 

Bài toán xác định phép so sánh tổng thứ tự trên hai giá trị với trường hợp đặc biệt về đẳng thức. Vì mỗi cặp số nguyên phải thỏa mãn đúng một trong`a == b`,`a > b`, hoặc`a < b`, logic phân nhánh sẽ phân vùng không gian đầu vào hoàn toàn mà không bị chồng chéo. Mỗi nhánh tương ứng với chính xác một kết quả hợp lệ, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    if a == b:
        print("Pingju")
    elif a > b:
        print("Liang")
    else:
        print("Chuang")

if __name__ == "__main__":
    solve()
```Giải pháp đọc một dòng đầu vào và phân tích cú pháp ngay lập tức hai số nguyên. Thứ tự so sánh rất quan trọng: sự đẳng thức phải được kiểm tra trước tiên, vì đây là trường hợp duy nhất không phù hợp với phân loại bất đẳng thức nghiêm ngặt. 

Các chuỗi đầu ra được cố định và chỉ phụ thuộc vào kết quả so sánh. Không cần định dạng hoặc tính toán bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
```| Bước | một | b | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | a == b | Pingju | 

Điều này xác nhận nhánh bình đẳng. Cả hai giá trị đều khớp nhau, do đó thuật toán trả về kết quả hòa một cách chính xác. 

### Ví dụ 2 

đầu vào:```
7 2
```| Bước | một | b | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 2 | a != b | - | 
| 2 | 7 | 2 | a > b | Lương | 

Điều này thể hiện nhánh lớn hơn. Giá trị đầu tiên chiếm ưu thế, do đó người chiến thắng chính xác sẽ được tạo ra. 

### Ví dụ 3 

đầu vào:```
1 9
```| Bước | một | b | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 9 | a != b | - | 
| 2 | 1 | 9 | một < b | Trang | 

Điều này xác nhận trường hợp dự phòng cuối cùng khi giá trị thứ hai lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một phép so sánh giữa hai số nguyên | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Việc tính toán cho mỗi trường hợp thử nghiệm là không đổi, do đó, ngay cả số lượng lớn các trường hợp cũng sẽ không đáng kể trong các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    a, b = map(int, input().split())
    if a == b:
        return "Pingju"
    elif a > b:
        return "Liang"
    else:
        return "Chuang"

# sample-like cases
assert run("3 3") == "Pingju"
assert run("7 2") == "Liang"
assert run("1 9") == "Chuang"

# custom edge cases
assert run("0 0") == "Pingju"
assert run("-1 -5") == "Liang"
assert run("-10 -2") == "Chuang"
assert run("1000000000 999999999") == "Liang"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | Pingju | xử lý bình đẳng | 
| -1 -5 | Lương | so sánh số âm | 
| 1 9 | Trang | nhánh cơ bản ít hơn | 
| 0 0 | Pingju | trường hợp cạnh bằng không | 
| 1e9 1e9-1 | Lương | ranh giới giá trị lớn | 

## Vỏ cạnh 

Để bình đẳng, thuật toán kiểm tra rõ ràng`a == b`trước bất kỳ logic bất đẳng thức nào. Đối với đầu vào`5 5`, việc thực thi dừng ngay lập tức ở nhánh đẳng thức và in`"Pingju"`. 

Đối với số âm, hãy xem xét đầu vào`-3 -10`. Việc so sánh vẫn hoạt động bình thường:`-3 > -10`, do đó thuật toán đạt đến nhánh lớn hơn và kết quả đầu ra`"Liang"`. Không cần xử lý đặc biệt vì thứ tự số nguyên cũng là tổng trên số âm. 

Đối với các giá trị lớn như`10^9 10^9`, đẳng thức vẫn kích hoạt nhánh đầu tiên, đảm bảo hành vi vẽ chính xác ngay cả ở cường độ biên.
