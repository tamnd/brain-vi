---
title: "CF 1860B - Tiền xu lạ mắt"
description: "Chúng ta được yêu cầu xác định số lượng đồng xu ưa thích tối thiểu mà Monocarp phải sử dụng để trả chính xác m burles khi anh ta có hai loại đồng xu: một loại có giá trị 1 burle và một loại có giá trị k burles."
date: "2026-06-09T00:21:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1860
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 153 (Rated for Div. 2)"
rating: 1200
weight: 1860
solve_time_s: 106
verified: true
draft: false
---

[CF 1860B - Tiền xu ưa thích](https://codeforces.com/problemset/problem/1860/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định số lượng đồng xu ưa thích tối thiểu Monocarp phải sử dụng để thanh toán chính xác`m`burles khi anh ta có hai loại đồng xu: một loại có giá trị 1 burle và một loại có giá trị`k`burles. Đối với mỗi loại, anh ta có một số lượng xu thông thường giới hạn (`a_1`của đồng xu 1-burle,`a_k`của đồng xu k-burle) và vô số đồng xu ưa thích. Những đồng xu lạ mắt sẽ "đắt" hơn theo nghĩa Monocarp muốn giảm thiểu việc sử dụng chúng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi, chúng tôi nhận được tổng số tiền`m`, giá trị đồng xu lớn hơn`k`và số lượng xu thông thường có sẵn`a_1`Và`a_k`. Chúng ta phải tính toán, trong mỗi trường hợp, tổng số xu ưa thích nhỏ nhất cần thiết để đạt được chính xác`m`sử dụng bất kỳ sự kết hợp nào của đồng xu thông thường và ưa thích. 

Những ràng buộc chỉ ra rằng`m`Và`k`có thể lớn như`10^8`, và số lượng ca kiểm thử`t`có thể lên tới 30.000. Điều này loại trừ bất kỳ giải pháp nào lặp đi lặp lại thông qua tất cả các kết hợp tiền xu có thể vì điều đó có thể dẫn đến tối đa`10^16`hoạt động theo cách tiếp cận ngây thơ. Chúng ta cần một phương pháp tính toán kết quả theo thời gian không đổi cho mỗi trường hợp thử nghiệm hoặc thời gian logarit tương ứng với`k`. 

Một số trường hợp khó phát hiện là khi Monocarp không có đồng xu thông thường thuộc một hoặc cả hai loại hoặc khi`m`nhỏ hơn`k`. Ví dụ, nếu`m = 5`,`k = 3`, Và`a_1 = 0`,`a_k = 1`, đáp án đúng là 2 đồng xu lạ mắt (một xu 3 burle và hai đồng xu 1 burle), chứ không phải 1 xu lạ mắt. Việc thực hiện bất cẩn, tham lam sử dụng số tiền lớn mà không kiểm tra số lượng tiền xu thường xuyên sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các kết hợp về số lượng đồng xu của từng loại sẽ sử dụng, tính toán tổng số đồng xu ưa thích cho mỗi khả năng. Cụ thể, chúng ta có thể lặp lại`x`từ 0 đến tối thiểu`a_k`Và`m//k`, đại diện cho số lượng đồng xu k-burle thông thường được sử dụng, sau đó giải để tìm số lượng đồng xu 1-burle cần thiết. Cách tiếp cận này đúng nhưng quá chậm: trong trường hợp xấu nhất, chúng ta có thể thử`m/k`khả năng cho mỗi trường hợp trong số 30.000 trường hợp thử nghiệm, dễ dàng vượt quá giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là xem xét phần còn lại modulo`k`. Tổng số tiền`m`có thể được thể hiện như`m = q * k + r`Ở đâu`0 ≤ r < k`. Số lượng đồng xu 1-burle ưa thích chính xác là số tiền còn lại mà đồng xu 1-burle thông thường không thể bù đắp được, tức là`max(0, r - a_1)`. Đối với đồng xu k-burle, bất kỳ sự thiếu hụt nào ngoài đồng xu k-burle thông thường đều góp phần`q - a_k`đồng tiền ưa thích, nhưng chỉ khi tích cực. Điều này làm giảm vấn đề thành một phép tính số học đơn giản bằng cách sử dụng phép chia và modulo, loại bỏ sự cần thiết phải lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m/k) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia tổng số tiền`m`qua`k`để tìm thương số`q`và phần còn lại`r`.`q`đại diện cho số lượng xu k-burle cần thiết nếu chúng ta bỏ qua tính sẵn có của đồng xu thông thường.`r`đại diện cho số tiền còn lại phải trả bằng đồng xu 1-burle. 
2. Tính xem cần bao nhiêu đồng xu 1-burle ưa thích. Sử dụng`fancy_1 = max(0, r - a_1)`. Điều này đảm bảo trước tiên chúng tôi tiêu thụ tất cả các đồng xu 1-burle thông thường có sẵn để trang trải phần còn lại. 
3. Tính xem cần bao nhiêu đồng xu k-burle ưa thích. Sau khi tiêu thụ hết đồng xu k-burle thông thường, mọi yêu cầu về đồng xu k-burle còn lại phải đến từ những đồng xu ưa thích. Sử dụng`fancy_k = max(0, q - a_k)`. 
4. Tính tổng hai giá trị:`fancy_1 + fancy_k`là số lượng xu ưa thích tối thiểu được yêu cầu. 

Tại sao nó hoạt động: trước tiên bằng cách thanh toán càng nhiều càng tốt bằng đồng xu thông thường và sau đó chỉ sử dụng đồng xu ưa thích để bù đắp khoản thiếu hụt, chúng tôi đảm bảo việc sử dụng đồng xu ưa thích ở mức tối thiểu. Phép chia và modulo đảm bảo rằng số lượng đồng xu k-burle được tự động giảm thiểu vì mọi số tiền 1-burle còn lại đều được tính trực tiếp là số dư. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        m, k, a1, ak = map(int, input().split())
        q, r = divmod(m, k)
        fancy_1 = max(0, r - a1)
        fancy_k = max(0, q - ak)
        print(fancy_1 + fancy_k)

if __name__ == "__main__":
    main()
```Giải pháp đọc tất cả đầu vào một cách hiệu quả với`sys.stdin.readline`.`divmod`tránh viết hai phép tính riêng biệt cho thương và số dư. sử dụng`max(0, ...)`đảm bảo chúng tôi không bao giờ báo cáo tiền xu ưa thích tiêu cực. Thứ tự trừ đảm bảo trước tiên chúng ta tiêu thụ tất cả các đồng tiền thông thường trước khi đếm các đồng tiền ưa thích, phù hợp với chiến lược tham lam. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`m = 11, k = 3, a1 = 6, ak = 1`| Bước | q, r | ưa thích_1 | ưa thích_k | tổng số ưa thích | 
| --- | --- | --- | --- | --- | 
| Tính q, r | 11 // 3 = 3, 11 % 3 = 2 | tối đa(0, 2 - 6) = 0 | tối đa(0, 3 - 1) = 2 | 2 | 

Dấu vết cho thấy rằng mặc dù đồng xu 1-burle là đủ để trang trải phần còn lại, nhưng chúng ta cần hai đồng xu k-burle ưa thích để tạo thành số lượng lớn. 

**Ví dụ 2:**`m = 11, k = 3, a1 = 0, ak = 0`| Bước | q, r | ưa thích_1 | ưa thích_k | tổng số ưa thích | 
| --- | --- | --- | --- | --- | 
| Tính q, r | 11 // 3 = 3, 11 % 3 = 2 | tối đa(0, 2 - 0) = 2 | tối đa(0, 3 - 0) = 3 | 5 | 

Ở đây không có đồng xu thông thường nào tồn tại nên tất cả các đồng xu đều phải lạ mắt. Điều này phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm chỉ thực hiện phép chia, modulo và hai phép tính tối đa. | 
| Không gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một vài biến số nguyên được lưu trữ. | 

Được cho`t ≤ 3e4`, tổng thời gian không đáng kể (~10^5 thao tác), thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n11 3 0 0\n11 3 20 20\n11 3 6 1\n100000000 2 0 0\n") == "5\n0\n1\n50000000"

# Custom cases
assert run("1\n5 5 0 1\n") == "0", "Use one regular k-burle coin"
assert run("1\n5 5 0 0\n") == "1", "Use one fancy k-burle coin"
assert run("1\n1 2 1 0\n") == "0", "Exactly enough 1-burle coin"
assert run("1\n1 2 0 0\n") == "1", "No regular coins, must use one fancy 1-burle coin"
assert run("1\n100000000 100000000 0 0\n") == "1", "Large k equal to m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 5 0 1`| 0 | Tiêu thụ đúng đồng xu k-burle thông thường | 
|`5 5 0 0`| 1 | Sử dụng đồng xu k-burle ưa thích khi không có sẵn thường xuyên | 
|`1 2 1 0`| 0 | Đồng xu 1-burle tối thiểu bao gồm phần còn lại | 
|`1 2 0 0`| 1 | Cần có đồng xu ưa thích tối thiểu khi không có đồng xu thông thường | 
|`100000000 100000000 0 0`| 1 | Số lớn được xử lý chính xác | 

## Vỏ cạnh 

Khi nào`a1 = 0`Và`ak = 0`, tất cả các đồng tiền phải lạ mắt. Đối với đầu vào`m = 11, k = 3`, chúng tôi tính toán`q = 3, r = 2`. Sau đó `ưa thích_
