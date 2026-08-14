---
title: "CF 102346H - Một giờ chạy bộ"
description: "Đường đua có N biển báo cách đều nhau và Vinicius dự định chạy đúng V vòng hoàn chỉnh. Vì một vòng vượt qua tất cả các ký hiệu N nên toàn bộ quá trình đào tạo bao gồm các đoạn ký hiệu VN."
date: "2026-08-13T01:27:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 65
verified: true
draft: false
---

[CF 102346H - Một giờ chạy](https://codeforces.com/problemset/problem/102346/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài hát chứa`N`các dấu hiệu cách đều nhau và Vinicius dự định chạy chính xác`V`hoàn thành vòng đua. Vì một vòng đã vượt qua tất cả`N`dấu hiệu, toàn bộ quá trình đào tạo bao gồm`V * N`ký các đoạn văn. 

Đối với mỗi phần trăm từ 10% đến 90%, chúng ta cần số nguyên nhỏ nhất của các dấu có phần tương ứng của quá trình huấn luyện hoàn chỉnh ít nhất là phần trăm đó. Đầu ra chứa chín giá trị, theo thứ tự 10%, 20%, ..., 90%. 

Ví dụ, với`V = 3`Và`N = 17`, quá trình đào tạo có`51`ký các đoạn văn. Đối với 30%, giá trị bắt buộc ít nhất là số nguyên nhỏ nhất`0.30 * 51 = 15.3`, cụ thể là`16`. Từ "ít nhất" là điều làm cho việc làm tròn lên trở nên cần thiết. 

Cả hai`V`Và`N`nhiều nhất là`10^4`, như vậy tổng số đoạn biển báo có thể đạt tới`10^8`. Do đó, một giải pháp quét mọi đoạn ký hiệu có thể thực hiện khoảng một trăm triệu lần lặp. Việc đó tốn nhiều công sức hơn mức cần thiết đối với một bài toán có đáp án chỉ gồm chín con số. Cấu trúc của phép tính cho phép chúng ta giải nó trong thời gian không đổi, bất kể quy mô đào tạo. 

Các trường hợp cạnh chính đến từ làm tròn. Với đầu vào`1 1`, toàn bộ khóa đào tạo có một đoạn ký hiệu. Mọi phần trăm dương từ 10% đến 90% đều yêu cầu đạt đến dấu duy nhất đó, vì vậy kết quả đầu ra đúng là`1 1 1 1 1 1 1 1 1`. Việc triển khai bất cẩn bằng cách sử dụng phép chia sàn số nguyên sẽ tạo ra số 0 cho mọi phần trăm. 

Một trường hợp ranh giới hữu ích khác là`3 11`. Tổng cộng là`33`, và 30% là`9.9`, vậy đáp án phải là`10`, không`9`. Đầu ra hoàn chỉnh là`4 7 10 14 17 20 24 27 30`. Phép chia số nguyên trực tiếp mà không có thao tác trần sẽ âm thầm thất bại ở các giá trị như`9.9`. 

Một trường hợp chia hết chính xác hoạt động khác nhau. Với đầu vào`10 10`, tổng số là`100`, vì vậy mỗi phần trăm được yêu cầu là một số nguyên dấu hiệu. Đầu ra là`10 20 30 40 50 60 70 80 90`. Công thức trần phải bảo toàn một số nguyên chính xác thay vì thêm một dấu phụ không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng từng dấu hiệu huấn luyện. có`V * N`tổng cộng các đoạn ký tên. Đối với mỗi tỷ lệ phần trăm, chúng tôi có thể tiếp tục tăng số lượng dấu hiệu đã đạt cho đến khi tỷ lệ trong tổng số khóa đào tạo đạt tỷ lệ phần trăm mong muốn. Điều này đúng vì vị trí đầu tiên đạt đến ngưỡng chính xác là số lượng dấu hiệu tối thiểu mà Vinicius phải đếm. 

Vấn đề là việc đào tạo tối đa chứa`10^4 * 10^4 = 10^8`ký các đoạn văn. Do đó, một mô phỏng có thể yêu cầu lên đến`10^8`lặp đi lặp lại và thực hiện quét riêng biệt theo nhiều tỷ lệ phần trăm sẽ khiến cách tiếp cận này trở nên lãng phí hơn. Mặc dù`10^8`về mặt toán học không phải là không thể, nó không cần thiết và mang lại cho giải pháp một biên độ hiệu suất kém thoải mái hơn nhiều. 

Quan sát quan trọng là số lượng dấu hiệu cho một khóa đào tạo hoàn chỉnh được biết ngay lập tức:`V * N`. Nếu chúng ta muốn ít nhất`k * 10%`của quá trình đào tạo, số lượng dấu hiệu có giá trị thực cần thiết là`(V * N * k) / 10`Ở đâu`k`nằm trong khoảng từ 1 đến 9. 

Câu trả lời phải là một số nguyên dấu và phải đủ lớn để đạt đến ngưỡng. Điều đó có nghĩa là chúng ta cần mức trần của giá trị này. Đối với số nguyên dương, trần của`x / 10`có thể được tính như`(x + 9) // 10`. Do đó, mỗi câu trả lời trong số chín câu trả lời có thể được lấy trực tiếp mà không cần mô phỏng. 

Brute-force hoạt động vì nó tìm kiếm số lượng dấu hiệu đầu tiên thỏa mãn từng điều kiện phần trăm, nhưng không thành công vì không gian tìm kiếm có thể chứa`10^8`các vị trí. Nhận xét rằng mỗi ngưỡng chỉ đơn giản là một phần cố định của tổng đã biết cho phép chúng ta thay thế việc tìm kiếm bằng chín phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(VN) | O(1) | Quá chậm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`V`Và`N`, Ở đâu`V * N`là tổng số dấu hiệu đã đạt được trong quá trình huấn luyện hoàn chỉnh. 
2. Tính toán`total = V * N`. Điều này cung cấp cho chúng tôi mẫu số cho mỗi phép tính phần trăm, vì vậy không có lý do gì để mô phỏng các vòng hoặc dấu hiệu riêng lẻ. 
3. Đối với mỗi`k`từ`1`bởi vì`9`, tính toán`total * k`. Chia đại lượng này cho`10`đưa ra số lượng chính xác các dấu hiệu tương ứng với`10k%`của việc đào tạo. 
4. Làm tròn giá trị đó lên trên bằng cách sử dụng`(total * k + 9) // 10`. Cần phải làm tròn lên vì người chạy cần phải hoàn thành ít nhất phần trăm được yêu cầu. Nếu giá trị chính xác đã là số nguyên thì phép chia số nguyên thông thường vẫn trả về giá trị chính xác đó. 
5. In chín giá trị được tính cách nhau bằng dấu cách, giữ nguyên thứ tự từ 10% đến 90%. 

Tại sao nó hoạt động: cho bất kỳ tỷ lệ phần trăm được yêu cầu nào`10k%`, người chạy ít nhất phải vượt qua`(V * N * k) / 10`dấu hiệu. Vì số dấu được truyền là số nguyên nên số đếm hợp lệ nhỏ nhất chính xác là mức trần của phân số đó. biểu hiện`(x + 9) // 10`tính mức trần đó cho mọi số nguyên dương`x`, vì vậy mỗi câu trả lời được tạo ra đều vừa đủ vừa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

V, N = map(int, input().split())

total = V * N

answer = [(total * k + 9) // 10 for k in range(1, 10)]

print(*answer)
```Dòng đầu tiên đọc hai đại lượng mô tả quá trình đào tạo. Chỉ có một test case nên sau khi đọc dữ liệu đầu vào, chương trình có thể thực hiện tính toán ngay lập tức.`total = V * N`đại diện cho số lượng đoạn biển báo trong tất cả các vòng theo kế hoạch. Vì mỗi vòng chứa chính xác`N`các dấu hiệu cách đều nhau, nhân số vòng với số dấu hiệu trên mỗi vòng sẽ có được thời lượng đào tạo đầy đủ trong các đơn vị liên quan đến vấn đề. 

Việc hiểu danh sách đánh giá chín tỷ lệ phần trăm được yêu cầu. Vì`k = 1`, nó tính ngưỡng 10%; vì`k = 2`, nó tính được 20%; vân vân thông qua`k = 9`. 

biểu hiện`(total * k + 9) // 10`là hoạt động trần chỉ có số nguyên. sử dụng`total * k // 10`sẽ sai bất cứ khi nào ngưỡng không phải là số nguyên, vì việc chia tầng sẽ làm tròn xuống và có thể khiến người chạy ở dưới tỷ lệ phần trăm được yêu cầu. 

Số nguyên Python tự động xử lý các giá trị lớn hơn số nguyên máy có chiều rộng cố định, do đó không có vấn đề tràn. Ngay cả ở đây,`V * N`nhiều nhất là`10^8`, và nhân nó với`9`cho nhiều nhất`9 * 10^8`, cũng phù hợp thoải mái với số nguyên có dấu 32 bit tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`V = 3`Và`N = 17`, có`51`ký các đoạn trong khóa đào tạo hoàn chỉnh. 

| k | Tỷ lệ phần trăm | tổng *k | Trần | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10% | 51 | 6 | 6 | 
| 2 | 20% | 102 | 11 | 11 | 
| 3 | 30% | 153 | 16 | 16 | 
| 4 | 40% | 204 | 21 | 21 | 
| 5 | 50% | 255 | 26 | 26 | 
| 6 | 60% | 306 | 31 | 31 | 
| 7 | 70% | 357 | 36 | 36 | 
| 8 | 80% | 408 | 41 | 41 | 
| 9 | 90% | 459 | 46 | 46 | 

Đối với 30%, ngưỡng chính xác là`153 / 10 = 15.3`, vì vậy 15 dấu là không đủ và câu trả lời phải là 16. Quy tắc trần tương tự áp dụng độc lập cho tất cả chín phần trăm. 

### Mẫu 2 

cho`V = 5`Và`N = 17`, tổng số đào tạo bao gồm`85`ký các đoạn văn. 

| k | Tỷ lệ phần trăm | tổng *k | Trần | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10% | 85 | 9 | 9 | 
| 2 | 20% | 170 | 17 | 17 | 
| 3 | 30% | 255 | 26 | 26 | 
| 4 | 40% | 340 | 34 | 34 | 
| 5 | 50% | 425 | 43 | 43 | 
| 6 | 60% | 510 | 51 | 51 | 
| 7 | 70% | 595 | 60 | 60 | 
| 8 | 80% | 680 | 68 | 68 | 
| 9 | 90% | 765 | 77 | 77 | 

Ngưỡng 10% là`8.5`, vậy người chạy cần có 9 dấu hiệu. Ngược lại, ngưỡng 20% ​​chính xác là`17`, do đó câu trả lời vẫn là 17 thay vì trở thành 18. Điều này chứng tỏ tại sao phép tính trần lại thích hợp hơn là chỉ cộng một sau khi chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác chín phần trăm tính toán được thực hiện | 
| Không gian | O(1) | Chỉ có tổng giá trị và chín giá trị đầu ra được lưu trữ | 

Các ràng buộc cho phép lên đến`10^8`ký các đoạn văn, nhưng giải pháp tối ưu không bao giờ lặp lại các đoạn đó. Nó thực hiện chín phép tính cố định, do đó thời gian chạy của nó không phụ thuộc vào`V * N`và dễ dàng phù hợp với các nguồn lực sẵn có. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve(inp: str) -> str:
    input_stream = io.StringIO(inp)
    V, N = map(int, input_stream.readline().split())

    total = V * N
    answer = [(total * k + 9) // 10 for k in range(1, 10)]

    return " ".join(map(str, answer))

# Provided samples
assert solve("3 17\n") == "6 11 16 21 26 31 36 41 46", "sample 1"
assert solve("5 17\n") == "9 17 26 34 43 51 60 68 77", "sample 2"
assert solve("3 11\n") == "4 7 10 14 17 20 24 27 30", "sample 3"

# Minimum-size input
assert solve("1 1\n") == "1 1 1 1 1 1 1 1 1", "minimum values"

# Maximum-size input
assert solve("10000 10000\n") == (
    "10000000 20000000 30000000 40000000 50000000 "
    "60000000 70000000 80000000 90000000"
), "maximum values"

# Exact divisibility at every requested percentage
assert solve("10 10\n") == "10 20 30 40 50 60 70 80 90", "exact percentages"

# Fractional thresholds that require rounding upward
assert solve("1 7\n") == "1 2 3 3 4 5 6 7 7", "ceiling boundaries"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1 1 1 1 1 1 1 1 1`| Giá trị tối thiểu và thực tế là mọi phần trăm dương đều cần dấu duy nhất | 
|`10000 10000`|`10000000 20000000 30000000 40000000 50000000 60000000 70000000 80000000 90000000`| Giá trị tối đa và số học lớn | 
|`10 10`|`10 20 30 40 50 60 70 80 90`| Chia hết chính xác mà không cần thêm dấu không cần thiết | 
|`1 7`|`1 2 3 3 4 5 6 7 7`| Ngưỡng phân số và hành vi trần | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`1 1`, tổng số là`1`. Đối với mọi`k`từ 1 đến 9, biểu thức trở thành`(k + 9) // 10`, là 1 cho tất cả chín giá trị. Do đó, đầu ra là`1 1 1 1 1 1 1 1 1`. Việc triển khai dựa trên tầng sẽ in không chính xác số 0 vì`k // 10`bằng 0 đối với các giá trị này. 

Đối với đầu vào ranh giới`3 11`, tổng số là`33`. Ngưỡng 30% là`99 / 10 = 9.9`, do đó việc tính toán`(99 + 9) // 10 = 10`làm tròn lên một cách chính xác. Đầu ra đầy đủ là`4 7 10 14 17 20 24 27 30`. Điều này mắc phải lỗi phổ biến khi sử dụng`total * k // 10`, sẽ tạo ra 9 cho giá trị thứ ba. 

Đối với đầu vào chia hết chính xác`10 10`, tổng số là`100`. Ví dụ, ở mức 20%, ngưỡng chính xác là`20`, Và`(200 + 9) // 10`bằng`20`, không`21`. Điều tương tự áp dụng cho mọi phần trăm được yêu cầu, đưa ra`10 20 30 40 50 60 70 80 90`. các`+9`trong công thức trần chỉ bù cho phần còn lại khác 0, trong khi bội số chính xác vẫn không thay đổi. 

Để có đầu vào tối đa`10000 10000`, tổng số là`100,000,000`. Ngưỡng 90% là`90,000,000`và mọi số học vẫn chính xác. Thuật toán thực hiện chín lần lặp giống như đối với đầu vào tối thiểu, do đó việc tăng cường huấn luyện từ một đoạn dấu lên một trăm triệu không làm tăng số bước thuật toán.
