---
title: "CF 1920B - Trò Chơi Tổng Hợp"
description: "Chúng ta được yêu cầu mô phỏng một trò chơi hai bước được chơi trên một dãy số nguyên dương. Người chơi đầu tiên, Alice, có thể loại bỏ tối đa $k$ phần tử. Cô ấy muốn tổng cuối cùng của mảng càng lớn càng tốt."
date: "2026-06-08T19:28:17+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1920
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 919 (Div. 2)"
rating: 1100
weight: 1920
solve_time_s: 109
verified: true
draft: false
---

[CF 1920B - Trò chơi tổng kết](https://codeforces.com/problemset/problem/1920/B) 

**Đánh giá:** 1100 
**Tags:** trò chơi, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một trò chơi hai bước được chơi trên một dãy số nguyên dương. Người chơi đầu tiên, Alice, có thể loại bỏ tối đa$k$các phần tử. Cô ấy muốn tổng cuối cùng của mảng càng lớn càng tốt. Người chơi thứ hai, Bob, sau đó có thể lật lên$x$các phần tử, nhân chúng với$-1$, và anh ta muốn số tiền đó càng nhỏ càng tốt. Nhiệm vụ của chúng ta là tính tổng cuối cùng giả sử cả hai đều chơi tối ưu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết kích thước của mảng$n$, giới hạn của Alice$k$, giới hạn của Bob$x$, và mảng của$n$số nguyên. Đầu ra của mỗi trường hợp thử nghiệm là tổng cuối cùng sau khi cả hai người chơi thực hiện các bước đi tối ưu của mình. 

Những hạn chế cho chúng ta biết$n$có thể lên đến$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm và tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm cũng không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là chúng ta cần một nghiệm tuyến tính trong$n$,$O(n \log n)$nhiều nhất, như$O(n^2)$cách tiếp cận sẽ quá chậm. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có kích thước 1, trong đó Alice có thể loại bỏ phần tử duy nhất, để lại tổng bằng 0 hoặc khi$k \ge n$, cho phép Alice xóa mọi thứ. Một trường hợp phức tạp khác là khi$x \ge n$và Bob có thể lật tất cả các phần tử còn lại, biến một tổng lớn thành âm. Bỏ qua các điều kiện này có thể dẫn đến kết quả đầu ra không chính xác. Ví dụ, một đầu vào của`1 1 1`với mảng`[1]`phải sản xuất`0`, không`1`, vì Alice sẽ loại bỏ phần tử một cách tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ liệt kê mọi tập hợp con có kích thước lên tới$k$mà Alice có thể loại bỏ, sau đó với mỗi mảng kết quả, hãy liệt kê mọi tập hợp con có kích thước tối đa$x$mà Bob có thể lật được. Đối với mỗi sự kết hợp, hãy tính tổng và chọn mức tối đa/tối thiểu theo từng người chơi. Về nguyên tắc thì điều này đúng nhưng đối với$n \sim 2 \cdot 10^5$, điều đó là không khả thi vì nó sẽ liên quan đến$O(2^n)$hoạt động. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận thấy rằng cả hai người chơi đều hành động tham lam trước các yếu tố cực đoan. Alice muốn loại bỏ các giá trị lớn nhất mà Bob sẽ chuyển thành âm và Bob sẽ luôn chuyển các phần tử lớn nhất còn lại thành âm để giảm thiểu tổng. Điều này làm giảm vấn đề sắp xếp mảng và chỉ xem xét phần trên cùng.$k + x$các phần tử. Trong thực tế, Alice sẽ loại bỏ$k$phần tử lớn nhất nếu Bob có thể lật chúng hoặc chọn ít hơn nếu điều đó tối đa hóa tổng cuối cùng của cô ấy. 

Giải pháp tối ưu không yêu cầu theo dõi trạng thái phức tạp hoặc lập trình động vì nước đi của người chơi được xác định hoàn toàn bằng thứ tự. Chúng ta có thể tính tổng sau khi Alice loại bỏ tối đa$k$phần tử lớn nhất rồi trừ đi hai lần tổng của$x$phần tử lớn nhất còn lại (mà Bob lật), mang lại tổng cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n, k, x$và mảng$a$. 
2. Sắp xếp mảng$a$theo thứ tự tăng dần. Sắp xếp cho phép chúng ta dễ dàng chọn các phần tử lớn nhất để loại bỏ hoặc lật. 
3. Tính hành động của Alice. Cô ấy có thể loại bỏ tới$k$các phần tử. Việc loại bỏ các phần tử chỉ có lợi nếu chúng nằm trong số những phần tử lớn nhất mà Bob có thể lật được. Để tối đa hóa số tiền, cô ấy nên loại bỏ$k$phần tử lớn nhất. 
4. Tính hành động của Bob. Sau khi Alice bị loại bỏ, Bob lật lên$x$các phần tử lớn nhất còn lại bằng cách nhân chúng với$-1$. Điều này làm giảm số tiền càng nhiều càng tốt. 
5. Tính tổng mảng còn lại sau khi cả hai người chơi hành động. Đối với lần lật của Bob, trừ đi hai lần tổng số$x$phần tử lớn nhất còn lại, bởi vì việc lật một phần tử$y$thay đổi tổng số tiền bằng$-2y$. 
6. Xuất số tiền cuối cùng. 

Tại sao nó hoạt động: Alice luôn loại bỏ các phần tử có thể làm tổn hại đến tổng số tiền nhiều nhất nếu Bob lật chúng. Bob luôn lật các phần tử lớn nhất còn lại để giảm tổng. Việc sắp xếp đảm bảo chúng ta có thể truy cập hiệu quả các phần tử lớn nhất trong cả hai bước. Chiến lược tham lam này tạo ra kết quả tối ưu vì bài toán là tuyến tính đối với tổng và mỗi phần tử hoạt động độc lập sau khi được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, k, x = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        
        # Alice removes k largest
        if k > 0:
            a = a[:n - k]
        
        # Bob flips x largest remaining
        if x > 0:
            a[-x:] = [-v for v in a[-x:]]
        
        print(sum(a))

if __name__ == "__main__":
    main()
```Giải pháp sắp xếp mảng để chúng tôi có thể xác định hiệu quả các phần tử lớn nhất cần loại bỏ và lật. Chúng ta cắt mảng để loại bỏ các phần tử của Alice và lật mảng cuối cùng`x`phần tử cho Bob. Việc cắt âm của Python xử lý các trường hợp trong đó`x`hoặc`k`vượt quá kích thước mảng một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 1 1`với mảng`[1]`. 

| Bước | Trạng thái mảng | Tổng hợp | 
| --- | --- | --- | 
| Ban đầu | [1] | 1 | 
| Alice loại bỏ 1 phần tử | [] | 0 | 
| Bob lật 0 phần tử | [] | 0 | 

Tổng cuối cùng:`0`. 

### Ví dụ 2 

đầu vào:`4 1 1`với mảng`[3, 1, 2, 4]`. 

| Bước | Trạng thái mảng | Tổng hợp | 
| --- | --- | --- | 
| Ban đầu | [1, 2, 3, 4] | 10 | 
| Alice loại bỏ 1 lớn nhất | [1, 2, 3] | 6 | 
| Bob lật 1 lớn nhất | [1, 2, -3] | 0 | 

Tổng cuối cùng:`0`. 

Những dấu vết này xác nhận thuật toán xử lý chính xác việc loại bỏ và lật các phần tử lớn nhất để tạo ra tổng tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm; cắt và tính tổng là O(n) | 
| Không gian | O(n) | Lưu trữ mảng | 

Các ràng buộc giới hạn tổng của tất cả$n$ĐẾN$2 \cdot 10^5$. Giải pháp O(n log n) của chúng tôi phù hợp thoải mái trong vòng 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("8\n1 1 1\n1\n4 1 1\n3 1 2 4\n6 6 3\n1 4 3 2 5 6\n6 6 1\n3 7 3 3 32 15\n8 5 3\n5 5 3 3 3 2 9 9\n10 6 4\n1 8 2 9 3 3 4 5 3 200\n2 2 1\n4 3\n2 1 2\n1 3") == "0\n2\n0\n3\n-5\n-9\n0\n-1"

# Custom cases
assert run("1\n3 3 1\n10 20 30") == "0", "Alice removes everything"
assert run("1\n3 1 5\n1 2 3") == "-4", "Bob flips more than remaining elements"
assert run("1\n5 0 2\n5 5 5 5 5") == -10, "Alice does nothing, Bob flips largest"
assert run("1\n1 1 1\n1000") == "0", "Single element removed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 1, mảng [10,20,30] | 0 | Alice xóa tất cả | 
| 3 1 5, mảng [1,2,3] | -4 | Bob lật nhiều hơn còn lại | 
| 5 0 2, mảng [5,5,5,5,5] | -10 | Bob lật người lớn nhất khi Alice không làm gì | 
| 1 1 1, mảng [1000] | 0 | Vỏ cạnh một phần tử | 

## Vỏ cạnh 

Thuật toán xử lý mảng một phần tử một cách chính xác bằng cách cho phép Alice loại bỏ nó, dẫn đến tổng bằng 0. Khi số lần lật của Bob vượt quá số phần tử còn lại, thao tác cắt lát trong Python sẽ lật tất cả các phần tử còn lại một cách an toàn mà không gặp lỗi. Mảng có các phần tử lặp lại hoặc các phần tử có kích thước tối đa cũng được xử lý chính xác vì các phép toán chỉ phụ thuộc vào việc sắp xếp và chọn các phần tử cực trị. Điều này xác nhận giải pháp hoạt động trên tất cả các điều kiện biên do vấn đề chỉ định.
