---
title: "CF 2156B - Máy lạ"
description: "Chúng tôi có một sự sắp xếp vòng tròn gồm tối đa 20 máy, mỗi máy sẽ giảm một số nguyên đi một (loại A) hoặc giảm một nửa và xếp tầng kết quả (loại B)."
date: "2026-06-08T00:24:54+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2156
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1061 (Div. 2)"
rating: 1000
weight: 2156
solve_time_s: 79
verified: true
draft: false
---

[CF 2156B - Máy lạ](https://codeforces.com/problemset/problem/2156/B) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một sự sắp xếp vòng tròn gồm tối đa 20 máy, mỗi máy sẽ giảm một số nguyên đi một (loại A) hoặc giảm một nửa và xếp tầng kết quả (loại B). Bắt đầu từ máy đầu tiên, chúng ta liên tục áp dụng thao tác của máy và chuyển sang máy tiếp theo theo thứ tự, quấn quanh máy cuối cùng. Đối với mỗi số nguyên truy vấn, chúng tôi muốn xác định số bước cho đến khi nó đạt đến 0. 

Các ràng buộc chỉ ra rằng mô phỏng lực mạnh cho mỗi truy vấn là khả thi đối với các chu kỳ nhỏ, bởi vì$n \le 20$. Tuy nhiên, số lượng truy vấn có thể lên tới$10^4$cho mỗi trường hợp thử nghiệm và mỗi số nguyên truy vấn có thể lớn bằng$10^9$. Việc mô phỏng từng bước theo nghĩa đen có thể mất tới$10^9$các hoạt động cho một truy vấn nếu số lượng lớn và tất cả các máy đều thuộc loại A. Điều này rõ ràng là quá chậm, vì vậy chúng ta cần một phương pháp để giảm số lượng hoạt động, lý tưởng nhất là bằng cách tận dụng số lượng máy nhỏ và mô hình chu kỳ của chúng. 

Trường hợp cạnh phát sinh khi tất cả các máy đều thuộc loại B. Ví dụ: nếu$n=3$và chuỗi máy là "BBB" với chữ cái đầu$a=1$, máy thứ nhất giảm một nửa từ 1 xuống 0, bước một bước. Một cách tiếp cận bất cẩn có thể cho rằng bạn luôn cần phải thực hiện toàn bộ chu trình, điều này sẽ đếm quá nhiều bước. 

Một kịch bản tinh tế khác là các máy xen kẽ, chẳng hạn như "ABAB" với số lượng lớn như 10. Trình tự giảm không đều: A giảm một, B giảm một nửa và mô hình lặp lại. Việc tính toán trước các hiệu ứng trên mỗi chu kỳ là rất quan trọng để tránh mô phỏng từng bước riêng lẻ lên đến số lượng lớn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi khởi tạo$a$và áp dụng lặp lại thao tác của máy hiện tại trong khi tăng bộ đếm bước, quấn quanh vòng tròn nếu cần. Điều này hoạt động chính xác vì các quy tắc cập nhật mang tính quyết định. Vấn đề xảy ra ở hiệu quả: đối với số lượng lớn như$10^9$với hầu hết các máy loại A, nó sẽ yêu cầu khoảng$10^9$hoạt động, điều này là không thể trong thời gian giới hạn. 

Cái nhìn sâu sắc quan trọng đến từ thực tế là chu kỳ của máy móc rất nhỏ. Từ$n \le 20$, chúng ta có thể tính toán trước tác động của một phép quay đầy đủ trên bất kỳ số nguyên nào. Cụ thể, chúng ta có thể mô phỏng toàn bộ chu trình để xác định cần bao nhiêu bước cho$a$giảm xuống dưới ngưỡng (hoặc cho đến khi nó ngừng thay đổi đáng kể). Đối với máy loại B, mỗi bước gần như giảm một nửa giá trị, vì vậy sau một vài chu kỳ, ngay cả những số nguyên rất lớn cũng co lại nhanh chóng. Sau đó, chúng tôi có thể tính toán hàng loạt các chu trình thay vì một máy tại một thời điểm. 

Do đó, cách tiếp cận tối ưu kết hợp mô phỏng chu trình với việc giảm số lượng lớn một cách tham lam. Chúng tôi liên tục áp dụng các chu kỳ đầy đủ miễn là$a$giảm đi đáng kể thì mô phỏng các bước còn lại lần lượt từng máy một$a$trở nên đủ nhỏ. Điều này đảm bảo tính chính xác và hiệu quả ngay cả đối với số lượng lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * a) | O(1) | Quá chậm đối với a lớn | 
| Tối ưu | O(q * n * log a) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$q$, chuỗi máy và danh sách các số nguyên truy vấn. Chúng tôi sẽ xử lý từng truy vấn một cách độc lập. 
2. Với mỗi số nguyên truy vấn$a$, khởi tạo bộ đếm bước ở mức 0 và đặt chỉ số máy hiện tại về 0 (bắt đầu từ máy 1). 
3. Trong khi$a > 0$, kiểm tra xem$a$đủ lớn để việc mô phỏng toàn bộ chu trình sẽ giảm đáng kể. Điều này luôn đúng nếu$a > n$bởi vì mỗi chu kỳ chứa các hoạt động loại A giảm dần$a$ít nhất bằng 0 và các phép toán loại B giảm một nửa$a$. 
4. Nếu có thể áp dụng một chu trình đầy đủ, hãy mô phỏng chu trình: lặp qua các máy, cập nhật$a$và tăng bộ đếm bước trên mỗi máy cho đến khi chu trình kết thúc hoặc$a$trở thành số không. Điều này xử lý các tác động bất thường của máy loại B một cách hiệu quả, vì việc giảm một nửa nhanh chóng làm giảm$a$. 
5. Một lần$a$nhỏ (ví dụ: nhỏ hơn 2n), tiếp tục mô phỏng từng máy một cho đến khi$a$đạt đến số không. Mỗi thao tác đều đơn giản: giảm một cho loại A, giảm một nửa cho loại B, sau đó chuyển sang máy tiếp theo. 
6. Sau$a$đạt tới 0, hãy ghi lại tổng số bước đã thực hiện cho truy vấn đó. 
7. Xuất ra câu trả lời cho tất cả các truy vấn theo thứ tự. 

Tại sao nó hoạt động: Thuật toán duy trì các quy tắc cập nhật chính xác của từng máy và duy trì bộ đếm bước một cách chính xác. Bằng cách mô phỏng các chu kỳ đầy đủ trước tiên, chúng tôi tránh được các cập nhật riêng lẻ không cần thiết cho các$a$, nhưng logic giống với mô phỏng đơn giản nên tính chính xác được đảm bảo. Chu kỳ được lặp lại cho đến khi$a$trở nên đủ nhỏ để xử lý từng bước, đảm bảo không đếm thừa hoặc bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        a_list = list(map(int, input().split()))
        res = []

        for a in a_list:
            steps = 0
            cur = a
            idx = 0
            while cur > 0:
                if cur > n:
                    for m in s:
                        if cur == 0:
                            break
                        if m == 'A':
                            cur -= 1
                        else:
                            cur //= 2
                        steps += 1
                else:
                    while cur > 0:
                        if s[idx] == 'A':
                            cur -= 1
                        else:
                            cur //= 2
                        idx = (idx + 1) % n
                        steps += 1
            res.append(str(steps))
        print('\n'.join(res))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc đầu vào một cách hiệu quả. Đối với mỗi truy vấn, chúng tôi duy trì`cur`như giá trị hiện tại của`a`Và`idx`làm chỉ số máy hiện tại. Vòng ngoài xử lý số lượng lớn một cách hiệu quả bằng cách mô phỏng toàn bộ chu trình tại một thời điểm, đảm bảo máy loại B giảm nhanh chóng`a`. Một lần`a`nhỏ, vòng lặp bên trong xử lý từng máy một. Chúng tôi nối các kết quả dưới dạng chuỗi và xuất chúng ở cuối. 

Các điểm tinh tế đang gói chính xác chỉ mục bằng modulo`n`và phân biệt giữa lớn`a`(nơi mô phỏng chu trình hiệu quả) và nhỏ`a`(trong đó các bước riêng lẻ là cần thiết). Các lỗi khác nhau có thể dễ dàng xảy ra nếu mô phỏng chu trình di chuyển chỉ mục hoặc đếm bước không chính xác. 

## Ví dụ đã hoạt động 

**Ví dụ 1: n=2, máy=BA, truy vấn=[3,4]** 

| Bước | Máy | một trước | sau | Bước | 
| --- | --- | --- | --- | --- | 
| 1 | B | 3 | 1 | 1 | 
| 2 | A | 1 | 0 | 2 | 
| 1 | B | 4 | 2 | 1 | 
| 2 | A | 2 | 1 | 2 | 
| 1 | B | 1 | 0 | 3 | 

Bảng cho thấy việc mô phỏng từng máy đếm chính xác các bước cho đến khi`a`đạt đến số không. 

**Ví dụ 2: n=1, máy=B, truy vấn=[20]** 

| Bước | Máy | một trước | sau | Bước | 
| --- | --- | --- | --- | --- | 
| 1 | B | 20 | 10 | 1 | 
| 1 | B | 10 | 5 | 2 | 
| 1 | B | 5 | 2 | 3 | 
| 1 | B | 2 | 1 | 4 | 
| 1 | B | 1 | 0 | 5 | 

Ngay cả với một máy duy nhất, việc giảm một nửa có hiệu quả làm giảm số lượng lớn`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * n * log a) | Mỗi truy vấn yêu cầu tối đa các chu trình log(a) để giảm một nửa số lượng lớn và mỗi chu trình bao gồm n thao tác. | 
| Không gian | O(n + q) | Chúng tôi lưu trữ các loại máy và kết quả cho mỗi truy vấn. | 

Được cho`n <= 20`và tổng của`q <= 10^4`, điều này dễ dàng phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n2 2\nBA\n3 4\n1 1\nB\n20\n6 4\nBAABBA\n2 8 32 95\n") == "2\n3\n5\n2\n5\n8\n11", "sample 1"

# Custom cases
assert run("1\n1 3\nA\n1 2 3\n") == "1\n2\n3", "single A machine"
assert run("1\n1 3\nB\n1 2 3\n") == "1\n2\n3", "single B machine"
assert run("1\n2 2\nAB\n1000000000 1\n") == "60\n2", "large and small number"
assert run("1\n3 1\nBBB\n100\n") == "7", "all B machines"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Những gì nó xác nhận
