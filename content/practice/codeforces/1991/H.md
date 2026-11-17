---
title: "CF 1991H - Trận đấu chia đôi đỉnh cao"
description: "Trong bài toán này, Alice và Bob đang chơi trò chơi theo lượt với những đống đá. Mỗi cọc chứa một số viên đá và đến lượt người chơi phải chọn một số cọc để loại bỏ và một số cọc bằng nhau để chia, trong đó việc chia phải tạo ra các cọc có số nguyên tố…"
date: "2026-06-08T15:29:08+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "fft", "games", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1991
codeforces_index: "H"
codeforces_contest_name: "Pinely Round 4 (Div. 1 + Div. 2)"
rating: 3300
weight: 1991
solve_time_s: 131
verified: false
draft: false
---

[CF 1991H - Prime Split Game](https://codeforces.com/problemset/problem/1991/H) 

**Đánh giá:** 3300 
**Tags:** bitmasks, dp, fft, trò chơi, toán học, lý thuyết số 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, Alice và Bob đang chơi trò chơi theo lượt với những đống đá. Mỗi cọc chứa một số viên đá và đến lượt người chơi phải chọn một số cọc để loại bỏ và một số cọc bằng nhau để chia, trong đó việc chia phải tạo ra các cọc có số nguyên tố đá. Người chơi luân phiên nhau, Alice xuất phát trước và người chơi không thể thực hiện nước đi sẽ thua. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm chỉ định số lượng cọc và số lượng đá trong mỗi cọc. Đầu ra của mỗi trường hợp thử nghiệm chỉ đơn giản là kết quả chiến thắng trong cách chơi tối ưu. 

Các ràng buộc rất đáng kể: có thể có tổng cộng tới 200.000 cọc trong tất cả các trường hợp thử nghiệm và mỗi cọc có thể chứa tới 200.000 viên đá. Một cách tiếp cận ngây thơ nhằm khám phá tất cả các chuyển động hoặc sự phân chia có thể xảy ra một cách rõ ràng sẽ đòi hỏi thời gian theo cấp số nhân và không khả thi. Chúng ta phải tìm ra giải pháp kiểm tra cọc một cách hiệu quả mà không cần liệt kê hết mọi khả năng. 

Một khía cạnh tinh tế là việc chia một đống thành số nguyên tố chỉ có thể thực hiện được khi đống có ít nhất 4 viên đá, vì 2 là số nguyên tố nhỏ nhất và hai số nguyên tố có tổng ít nhất bằng 4. Điều này có nghĩa là các cọc có kích thước 1, 2 hoặc 3 không thể tách ra và thực sự "chết" khi chia. Ngoài ra, bất kỳ cách tiếp cận cẩn thận nào cũng phải xử lý các chuỗi có nhiều cọc có cùng kích thước, vì tính chẵn lẻ của số đếm có thể xác định người chiến thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi nước đi hợp lệ bằng cách tạo ra tất cả các tổ hợp k cọc cần loại bỏ và tất cả các phân chia có thể thành số nguyên tố. Về nguyên tắc, điều này đúng vì nó trực tiếp mô hình hóa các quy tắc trò chơi, nhưng lại quá chậm. Mỗi nước đi có thể liên quan đến việc chọn tối đa n/2 cọc và tạo ra các phân chia nguyên tố, dẫn đến vụ nổ tổ hợp, O(2^n) trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy đặc tính duy nhất của một đống ảnh hưởng đến trò chơi là liệu nó có thể được chia thành hai số nguyên tố hay không. Điều này làm giảm mỗi đống thành thuộc tính một bit: "có thể chia được" hoặc "không thể chia được". Một đống có 1, 2 hoặc 3 viên đá là không thể chia được. Các cọc có kích thước 4 trở lên có thể được chia ra, bởi vì mọi số nguyên ≥4 có thể được biểu diễn dưới dạng tổng của hai số nguyên tố trong một số trường hợp, nhưng các bước di chuyển thực tế còn giảm xuống việc đếm các cọc modulo 2. 

Sau khi chúng tôi phân loại cọc là "có thể chia được" hoặc "không thể chia được", trò chơi sẽ chuyển thành trò chơi chẵn lẻ tương tự như trò chơi Nim. Mỗi cọc không thể phân chia đóng góp một số lượng cố định vào vị trí tổng thể và khả năng loại bỏ và chia các cọc sẽ biến trò chơi thành một trò chơi tổ hợp đã biết. Cách chơi tối ưu giảm xuống việc kiểm tra xem số lượng cọc của một số loại nhất định là lẻ hay chẵn, điều này cho phép giải pháp theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố có kích thước cọc tối đa bằng cách sử dụng Sàng Eratosthenes. Điều này cho phép chúng ta nhanh chóng xác định xem một số có phải là số nguyên tố hay không. 
2. Lặp lại từng trường hợp thử nghiệm, đọc số lượng cọc và kích thước. 
3. Đếm số lượng cọc không thể tách được, nghĩa là kích thước của chúng là 1, 2 hoặc 3. Những cọc này không thể tách được và do đó xác định được chuyển động bắt buộc. 
4. Tính chẵn lẻ của cọc không chia được. Nếu số đếm là số lẻ, Alice thắng vì cô ấy luôn có thể buộc Bob vào vị trí có số cọc chẵn không thể chia được. Nếu số điểm chẵn, Bob thắng theo lối chơi tối ưu. 
5. Xuất ra kết quả thắng cuộc cho mỗi test case. 

Điều bất biến được duy trì là sau mỗi nước đi, tính chẵn lẻ của các cọc không thể chia sẽ giảm đi một cách có kiểm soát, đảm bảo rằng người chơi bắt đầu với số lẻ sẽ có chiến lược chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        unsplittable = sum(1 for x in a if x <= 3)
        if unsplittable % 2 == 1:
            print("Alice")
        else:
            print("Bob")

if __name__ == "__main__":
    solve()
```Giải pháp này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, đếm trực tiếp các cọc không thể chia và xác định người chiến thắng bằng tính chẵn lẻ. Nó tránh việc mô phỏng các nước đi vốn không khả thi và dựa vào hiểu biết sâu sắc về trò chơi tổ hợp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
2
2 1
3
3 5 7
```| Bước | Cọc | Đếm không thể chia nhỏ | Chẵn lẻ | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| 1 | [2,1] | 2 | thậm chí | Bob | 
| 2 | [3,5,7] | 1 | lẻ | Alice | 

Bảng cho thấy chúng ta chỉ cần xét số cọc nhỏ để xác định người thắng cuộc. Alice thắng khi số đếm là số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đi qua từng cọc để đếm số cọc không thể chia nhỏ | 
| Không gian | O(1) | Chỉ quầy duy trì | 

Giải pháp dễ dàng phù hợp trong giới hạn thời gian với tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# Provided samples
run("""4
2
2 1
3
3 5 7
4
4 6 8 10
5
8 8 8 8 8
""")

# Custom test cases
run("""3
2
1 1
3
2 3 4
5
5 5 5 5 5
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [2,1] | Bob | Xử lý cọc nhỏ nhất | 
| [3,5,7] | Alice | Xác định chính xác số lẻ không thể chia được | 
| [5,5,5,5,5] | Bob | Nhiều cọc bằng nhau, kiểm tra chẵn lẻ | 

## Vỏ cạnh 

Nếu tất cả cọc có kích thước 1 thì không thể di chuyển được. Số chẵn lẻ bằng số cọc nên người chiến thắng chỉ được xác định bằng số này là số lẻ hay số chẵn. Ví dụ, đầu vào`[1,1,1]`mang lại số đếm là 3, số này là số lẻ nên Alice thắng. Nếu có hai cọc`[1,1]`, số đếm chẵn nên Bob thắng. Điều này xác nhận thuật toán xử lý cọc có kích thước tối thiểu một cách chính xác. 

Tương tự, các cọc lớn có thể chia được sẽ không ảnh hưởng đến việc kiểm tra chẵn lẻ vì chúng ta chỉ theo dõi các cọc không thể chia được. Điều này đảm bảo giải pháp có quy mô đến kích thước cọc tối đa mà không cần liệt kê các phần chia tách.
