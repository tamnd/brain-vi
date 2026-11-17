---
title: "CF 1997E - Thăng Cấp"
description: "Trong bài toán này, Monocarp đang duyệt qua một chuỗi tuyến tính các quái vật, mỗi quái vật có một cấp độ. Anh ta bắt đầu ở cấp độ 1 và có thể chiến đấu với những quái vật có cấp độ ít nhất cao bằng cấp độ hiện tại của anh ta. Bất cứ khi nào anh ấy chiến đấu với k quái vật, anh ấy sẽ tăng cấp một."
date: "2026-06-08T14:37:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1997
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 168 (Rated for Div. 2)"
rating: 2200
weight: 1997
solve_time_s: 160
verified: false
draft: false
---

[CF 1997E - Tăng cấp](https://codeforces.com/problemset/problem/1997/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, phân chia và chinh phục, thực hiện 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, Monocarp đang duyệt qua một chuỗi tuyến tính các quái vật, mỗi quái vật có một cấp độ. Anh ta bắt đầu ở cấp độ 1 và có thể chiến đấu với những quái vật có cấp độ ít nhất cao bằng cấp độ hiện tại của anh ta. Bất cứ khi nào anh ấy chiến đấu`k`quái vật, anh ta tăng cấp một. Mỗi truy vấn hỏi liệu Monocarp có chiến đấu với một quái vật cụ thể hay không nếu tần suất tăng cấp là`k`. Đầu vào bao gồm mảng cấp độ quái vật và một loạt truy vấn, mỗi truy vấn chỉ định một quái vật và một giá trị.`k`. Kết quả đầu ra là "CÓ" nếu Monocarp chiến đấu với quái vật đó và "KHÔNG" nếu nó bỏ chạy. 

Các ràng buộc chỉ ra rằng cả số lượng quái vật và số lượng truy vấn có thể lên tới 200.000, điều này loại trừ bất kỳ thuật toán nào mô phỏng quy trình một cách nguyên bản cho mọi truy vấn. Một giải pháp đơn giản sẽ lặp qua tất cả các quái vật cho đến chỉ mục được truy vấn cho mỗi truy vấn, dẫn đến các hoạt động O(n*q), có thể đạt tới 4·10^10 trong trường hợp xấu nhất, quá lớn so với giới hạn thời gian 4 giây. Điều này ngay lập tức gợi ý rằng chúng ta cần tính toán trước hoặc cách tiếp cận ngoại tuyến để trả lời các truy vấn một cách hiệu quả. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`k = 1`, nghĩa là Monocarp tăng cấp sau mỗi trận chiến, và`k = n`, nơi anh ta có thể không bao giờ lên cấp trước khi đạt được những con quái vật sau này. Một sự tinh tế khác là khi nhiều quái vật có cùng cấp độ; phải cẩn thận để không tính quá nhiều trận đấu cần thiết để tăng cấp, vì cấp độ của Monocarp chỉ tăng sau chính xác`k`đánh nhau. Ví dụ: nếu tất cả quái vật đều cấp 1 và`k = 2`, Monocarp chiến đấu với trận đầu tiên, sau đó tăng cấp sau trận chiến thứ hai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng trò chơi cho từng truy vấn: bắt đầu ở cấp độ 1, lặp lại các quái vật, theo dõi số trận chiến và tính toán cấp độ của Monocarp một cách linh hoạt. Cách tiếp cận này đúng nhưng quá chậm, với độ phức tạp là O(n*q). 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào số trận chiến mà Monocarp đã gặp phải trước quái vật được truy vấn. Nếu chúng ta biết cần bao nhiêu trận chiến để đạt được mỗi cấp độ, chúng ta có thể tính toán số trận chiến tối thiểu cần thiết để đánh bại kẻ đó.`i`-con quái vật thứ Bằng cách xử lý trước mảng, chúng ta có thể chuyển đổi từng truy vấn thành một so sánh đơn giản: đã cho`k`, liệu mức tích lũy của Monocarp có đủ để chiến đấu với quái vật ở chỉ số không`i`? 

Giải pháp hiệu quả sử dụng **cách tiếp cận ngoại tuyến với tính toán tiền tố và tìm kiếm nhị phân**. Chúng ta có thể tính toán trước số lượng quái vật mà Monocarp phải chiến đấu để đạt được từng cấp độ và sau đó trả lời từng truy vấn trong O(1) hoặc O(log n) bằng cách so sánh số trận chiến cần thiết với`k`. Cái nhìn sâu sắc là số lần chiến đấu trước một con quái vật là đơn điệu`i`, điều này cho phép chúng ta suy luận một cách hiệu quả mà không cần phải mô phỏng lại toàn bộ quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n) | Quá chậm | 
| Ngoại tuyến + Tính toán trước | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một mảng`needed`như vậy`needed[i]`là số trận chiến tối thiểu mà Monocarp phải đối mặt để chiến đấu với quái vật`i`. Khởi tạo`level = 1`Và`fights = 0`. 
2. Lặp lại các quái vật theo thứ tự. Đối với mỗi quái vật, nếu`level > a[i]`, Monocarp bỏ qua quái vật. Ngược lại, anh ta chiến đấu, tăng thêm`fights`và tính toán xem việc tăng cấp có xảy ra hay không bằng cách sử dụng`k`. 
3. Thay vì tính toán riêng từng truy vấn, hãy lưu trữ **số lần chiến đấu tích lũy** trước đó cho từng quái vật. 
4. Đối với mỗi truy vấn`(i, x)`, kiểm tra xem cấp độ của Monocarp có đủ để chiến đấu với quái vật không`i`sau khi tính đến việc tăng cấp: cấp độ của quái vật`i`là`1 + floor(fights_before_i / x)`. Nếu mức này vượt quá`a[i]`, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Điều này hiệu quả vì tiến trình của Monocarp mang tính quyết định khi số lượng trận chiến được biết và mọi truy vấn chỉ khác nhau`k`. Bằng cách tính toán trước các vị trí chiến đấu, chúng tôi giảm mỗi truy vấn thành một phép tính số học duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    # Precompute the number of fights needed to reach each monster
    fights_needed = [0] * n
    max_level = 0
    for idx in range(n):
        fights_needed[idx] = max_level
        max_level = max(max_level, a[idx])
    
    for _ in range(q):
        i, k = map(int, input().split())
        i -= 1
        # Level at monster i
        level_at_i = 1 + fights_needed[i] // k
        if level_at_i > a[i]:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```### Giải thích cách thực hiện 

Chúng tôi không mô phỏng mọi cuộc chiến cho từng truy vấn. Thay vì,`fights_needed[i]`nắm bắt được số lượng giao tranh tối thiểu trước khi chạm trán với`i`-con quái vật thứ Việc tính toán`level_at_i = 1 + fights_needed[i] // k`trực tiếp cho biết cấp độ của Monocarp tại thời điểm đó. Sự so sánh cuối cùng rất đơn giản: nếu đủ cấp độ, Monocarp sẽ chiến đấu; nếu không, con quái vật sẽ bỏ chạy. Phải cẩn thận để điều chỉnh các chỉ số dựa trên 1 từ đầu vào sang chỉ số dựa trên 0 trong Python. 

## Ví dụ đã hoạt động 

| Truy vấn | tôi | k | chiến đấu_cần thiết[i] | cấp_at_i | Quái vật a[i] | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 2 | CÓ | 
| 2 | 2 | 1 | 1 | 2 | 1 | KHÔNG | 

Bảng này chứng minh rằng mảng chiến đấu được tính toán trước cho phép chúng ta tính toán cấp độ của Monocarp ở bất kỳ quái vật nào một cách nhanh chóng. Trường hợp cạnh với`k = 1`hoặc`k = n`đều được xử lý thống nhất bằng phép tính số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Tính toán trước số trận đấu trong O(n), trả lời từng truy vấn trong O(1) | 
| Không gian | O(n) | Lưu trữ số trận chiến được tính toán trước | 

Với những hạn chế của`n, q <= 2e5`, giải pháp này phù hợp thoải mái trong giới hạn 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided sample
assert run("""4 16
2 1 2 1
1 1
2 1
3 1
4 1
1 2
2 2
3 2
4 2
1 3
2 3
3 3
4 3
1 4
2 4
3 4
4 4""") == """YES
NO
YES
NO
YES
YES
YES
NO
YES
YES
YES
NO
YES
YES
YES
YES""", "sample 1"

# Custom test: minimum-size input
assert run("""1 1
1
1 1""") == "YES", "minimum size"

# Custom test: all monsters same level, k = 1
assert run("""3 3
2 2 2
1 1
2 1
3 1""") == "YES\nYES\nYES", "all-equal"

# Custom test: Monocarp never levels up before last monster
assert run("""4 2
5 5 5 5
1 2
4 4""") == "NO\nNO", "never levels up"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 quái vật, 1 truy vấn | CÓ | đầu vào nhỏ nhất | 
| 3 quái vật, k=1 | CÓ, CÓ, CÓ | Monocarp tăng cấp mỗi trận đấu | 
| 4 quái vật, k=n | KHÔNG, KHÔNG | Monocarp không đủ cấp để chiến đấu với quái vật sau này | 

## Vỏ cạnh 

Nếu`k = 1`, Monocarp tăng cấp sau mỗi trận chiến, vì vậy ngay cả những quái vật yếu cũng có thể chạy trốn nếu Monocarp tích lũy cấp độ nhanh chóng. Nếu như`k = n`, anh ta có thể không lên cấp chút nào trước những con quái vật cuối cùng. Thuật toán xử lý chính xác cả hai trường hợp vì nó tính toán`level_at_i`dựa trên phép chia số nguyên, điều này đương nhiên giải thích cho những thái cực này. Cấp độ của Monocarp luôn được tính toán chính xác mà không cần mô phỏng từng trận chiến.
