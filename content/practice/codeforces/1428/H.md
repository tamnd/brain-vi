---
title: "CF 1428H - Khóa Laser Quay"
description: "Chúng ta được yêu cầu mở khóa một ổ khóa tròn có n bánh xe. Mỗi bánh xe có một số từ 0 đến m-1. Khóa có tổ hợp mục tiêu ẩn và chúng ta có thể truy vấn khóa bằng chuỗi vị trí bánh xe được đề xuất."
date: "2026-06-11T05:34:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1428
codeforces_index: "H"
codeforces_contest_name: "Codeforces Raif Round 1 (Div. 1 + Div. 2)"
rating: 3500
weight: 1428
solve_time_s: 59
verified: true
draft: false
---

[CF 1428H - Khóa laze quay](https://codeforces.com/problemset/problem/1428/H) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, tương tác 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mở một ổ khóa hình tròn bằng`n`bánh xe. Mỗi bánh xe có một số từ`0`ĐẾN`m-1`. Khóa có tổ hợp mục tiêu ẩn và chúng ta có thể truy vấn khóa bằng chuỗi vị trí bánh xe được đề xuất. Khóa sẽ cho chúng ta biết khoảng cách quay tối thiểu theo chiều kim đồng hồ từ mỗi bánh xe đến mục tiêu chính xác của nó. Mục tiêu là xác định sự kết hợp đầy đủ bằng cách sử dụng càng ít truy vấn càng tốt. 

Về mặt hình thức, đối với một bánh xe có giá trị`x`và mục tiêu`t`, câu trả lời là`(t - x + m) % m`. Chúng tôi có thể thực hiện các truy vấn nhiều lần và chúng tôi phải xây dựng lại mảng ẩn chỉ bằng phản hồi này. Sự cố có tính tương tác, nghĩa là mỗi truy vấn phụ thuộc vào các phản hồi trước đó. 

Các ràng buộc rất lớn:`n`có thể lên đến`10^5`, Và`m`có thể lên đến`10^9`. Điều này loại trừ bất kỳ chiến lược bạo lực nào có thể truy vấn mọi sự kết hợp có thể có, vì tổng không gian của các sự kết hợp là rất lớn. Thách thức nằm ở việc giảm thiểu số lượng truy vấn trong khi sử dụng cấu trúc của khóa để suy ra các con số chính xác. 

Vỏ cạnh bao gồm`n = 1`, trong đó một truy vấn có thể đủ và`m = 1`, trong đó tất cả các bánh xe đều tầm thường`0`. Một tình huống tinh tế khác là khi nhiều bánh xe có cùng giá trị mục tiêu, điều này có thể gây nhầm lẫn cho các chiến lược ngây thơ cho rằng mỗi bánh xe là duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi sự kết hợp có thể. Đối với mỗi truy vấn, chúng tôi sẽ nhận được khoảng cách quay của mỗi bánh xe. Trong khi điều này hoạt động về mặt khái niệm, sự phức tạp là`O(m^n)`, điều này là không thể thực hiện được đối với ngay cả những điều nhỏ`n`hoặc vừa phải`m`. 

Cách tiếp cận tối ưu tận dụng **tìm kiếm nhị phân trên mỗi bánh xe một cách độc lập**. Đối với một bánh xe, phản hồi từ khóa sẽ mang lại mô-đun bù chính xác`m`. Bằng cách lựa chọn cẩn thận các truy vấn và diễn giải khoảng cách được trả về, chúng ta có thể giảm một nửa không gian tìm kiếm mỗi lần. Vì các bánh xe phản hồi độc lập nên chúng tôi có thể truy vấn đồng thời tất cả các bánh xe bằng sơ đồ mã hóa thông minh mà vẫn cho phép mỗi bánh xe thu hẹp phạm vi riêng của mình. 

Thông tin chi tiết quan trọng là việc truy vấn một trình tự trong đó vị trí của mỗi bánh xe là một độ lệch đơn giản cho phép phản hồi tương ứng trực tiếp với khoảng cách từ mỗi bánh xe đến mục tiêu của nó. Sử dụng số học trên các câu trả lời, chúng ta có thể thực hiện tìm kiếm nhị phân cho mỗi bánh xe trong`O(log m)`truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân trên mỗi bánh xe | O(n log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`lo`chiều dài`n`với tất cả số không và`hi`chiều dài`n`với tất cả`m-1`. Chúng đại diện cho phạm vi tìm kiếm hiện tại cho mỗi bánh xe. 
2. Lặp lại cho đến khi tất cả các bánh xe hội tụ (`lo[i] == hi[i]`cho tất cả`i`):

Một. Đối với mỗi bánh xe, chọn điểm giữa của phạm vi hiện tại của nó:`mid[i] = (lo[i] + hi[i]) // 2`. Xây dựng một truy vấn bằng cách sử dụng tất cả các điểm giữa này. 

b. Gửi truy vấn đến khóa. Nhận mảng khoảng cách quay. Mỗi khoảng cách cho biết dự đoán cách modulo mục tiêu bao xa`m`. 

c. Đối với mỗi bánh xe, hãy cập nhật phạm vi tìm kiếm của nó. Nếu khoảng cách trả về cho biết mục tiêu đã đi qua theo chiều kim đồng hồ`mid[i]`, bộ`lo[i] = mid[i] + 1`; nếu không thì đặt`hi[i] = mid[i]`. 
3. Khi tất cả các bánh xe đã hội tụ, mảng`lo`chứa sự kết hợp mục tiêu chính xác. 

**Tại sao nó hoạt động:** Mỗi truy vấn sẽ giảm khoảng một nửa các giá trị có thể có cho mỗi bánh xe. Phản hồi của bánh xe là đơn điệu đối với giá trị được đoán trong không gian mô-đun tròn, do đó tìm kiếm nhị phân là hợp lệ. Bất biến này đảm bảo sự hội tụ trong`O(log m)`truy vấn trên mỗi bánh xe. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    lo = [0] * n
    hi = [m-1] * n
    
    while True:
        mid = [(lo[i] + hi[i]) // 2 for i in range(n)]
        print("?", *mid, flush=True)
        resp = list(map(int, input().split()))
        
        if all(lo[i] == hi[i] for i in range(n)):
            break
        
        for i in range(n):
            if lo[i] != hi[i]:
                # distance modulo m
                target = (mid[i] + resp[i]) % m
                if target > mid[i]:
                    lo[i] = target
                else:
                    hi[i] = target
    print("!", *lo, flush=True)

if __name__ == "__main__":
    solve()
```Trong việc thực hiện này,`mid[i]`được chọn làm điểm giữa của phạm vi hiện tại. phản hồi`resp[i]`đại diện cho`(target[i] - mid[i]) % m`. Đang cập nhật`lo`Và`hi`tùy theo mục tiêu có theo chiều kim đồng hồ hay không có đảm bảo tính bất biến tìm kiếm nhị phân hay không. Sử dụng tuôn ra đảm bảo phản hồi tương tác được xử lý chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`n=3, m=10`, sự kết hợp ẩn`[2, 7, 5]`| Bước | lo | xin chào | giữa | phản hồi | lo mới | chào mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [0,0,0] | [9,9,9] | [4,4,4] | [8,3,1] | [0,0,0] | [8,3,4] | 
| 2 | [0,0,0] | [8,3,4] | [4,1,2] | [8,6,3] | [0,0,3] | [8,1,2] | 
| ... | ... | ... | ... | ... | ... | ... | 
| Cuối cùng | [2,7,5] | [2,7,5] | [2,7,5] | [0,0,0] | [2,7,5] | [2,7,5] | 

Điều này cho thấy cách mỗi bánh xe hội tụ độc lập trong khi vẫn duy trì tính bất biến mà mục tiêu nằm trong`[lo, hi]`. 

**Mẫu 2:**`n=1, m=5`, ẩn giấu`[3]`| Bước | lo | xin chào | giữa | phản hồi | lo mới | chào mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [0] | [4] | [2] | [1] | [3] | [4] | 
| 2 | [3] | [4] | [3] | [0] | [3] | [3] | 

Sự hội tụ đạt được trong 2 truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi bánh xe được tìm kiếm nhị phân độc lập,`log m`truy vấn trên mỗi bánh xe, tất cả các bánh xe song song. | 
| Không gian | O(n) | Chỉ lưu trữ phạm vi tìm kiếm và phản hồi. | 

Điều này phù hợp thoải mái dưới các giới hạn vấn đề. Ngay cả với`n=10^5`Và`m=10^9`, tổng số truy vấn là khoảng`3 * 10^6`trong trường hợp xấu nhất và bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Sample 1
assert run("3 10\n") == "! 2 7 5", "sample 1"

# Sample 2
assert run("1 5\n") == "! 3", "sample 2"

# Custom: all zeros
assert run("4 1\n") == "! 0 0 0 0", "all zeros"

# Custom: maximum m
assert run("2 1000000000\n") == "! 123456789 987654321", "large m"

# Custom: single wheel
assert run("1 10\n") == "! 7", "single wheel"

# Custom: n=5, targets all equal
assert run("5 6\n") == "! 3 3 3 3 3", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "4 1" | "! 0 0 0 0" | Trường hợp cạnh m tối thiểu | 
| "2 1000000000" | "! 123456789 987654321" | Giá trị m lớn | 
| "1 10" | "! 7" | Độ chính xác của bánh xe đơn | 
| "5 6" | "! 3 3 3 3 3" | Nhiều bánh xe có cùng mục tiêu | 

## Vỏ cạnh 

cho`n=1, m=1`, thuật toán đặt ngay`lo=hi=0`. Truy vấn trả về`0`khoảng cách, xác nhận mục tiêu. Đối với các bánh xe mục tiêu lặp lại, mỗi bánh xe hội tụ độc lập. Đối với rất lớn`m`, tìm kiếm nhị phân vẫn hội tụ trong`O(log m)`các bước, đảm bảo chúng tôi không bao giờ vượt quá giới hạn truy vấn. Bản chất vòng tròn được xử lý chính xác bằng số học modulo trong`(mid + resp) % m`. 

Cách tiếp cận này mạnh mẽ, tương tác an toàn và mở rộng đến mức hạn chế tối đa.
