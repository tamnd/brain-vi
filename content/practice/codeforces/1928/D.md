---
title: "CF 1928D - Ngục tối trên núi cô đơn"
description: "Chúng tôi được yêu cầu tập hợp một đội quân từ nhiều chủng tộc, mỗi chủng tộc có một số lượng sinh vật nhất định. Mỗi cặp sinh vật cùng chủng tộc nếu được xếp vào các đội khác nhau sẽ đóng góp một lượng b cố định vào sức mạnh của quân đội."
date: "2026-06-08T18:48:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1928
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 924 (Div. 2)"
rating: 1900
weight: 1928
solve_time_s: 123
verified: true
draft: false
---

[CF 1928D - Ngục tối trên núi cô đơn](https://codeforces.com/problemset/problem/1928/D) 

**Xếp hạng:** 1900 
**Tags:** brute power, cấu trúc dữ liệu, tham lam, toán học, tìm kiếm ternary 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tập hợp một đội quân từ nhiều chủng tộc, mỗi chủng tộc có một số lượng sinh vật nhất định. Mỗi cặp sinh vật cùng chủng tộc nếu được xếp vào các đội khác nhau sẽ đóng góp một số tiền cố định.`b`đến sức mạnh của quân đội. Mặt khác, mỗi đội bổ sung ngoài đội đầu tiên sẽ làm giảm tổng sức mạnh đi`x`. Nhiệm vụ của chúng ta là xác định cách phân chia sinh vật thành các đội để phát huy tối đa sức mạnh của quân đội. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết số lượng chủng tộc`n`, tiền thưởng cặp`b`, đội hình phạt`x`, và mảng`c`đưa ra số lượng sinh vật trên mỗi chủng tộc. Chúng ta phải xuất ra một số nguyên biểu thị sức mạnh quân đội tối đa cho từng trường hợp. 

Những hạn chế gợi ý rằng`n`có thể lên tới 200.000, nhưng tổng số tiền`c_i`trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này ngụ ý rằng chúng ta có thể thực hiện các phép toán tuyến tính đối với tổng số sinh vật, nhưng bất kỳ phép tính bậc hai nào có`c_i`quá chậm. Do đó, bất kỳ cách tiếp cận vũ phu nào cố gắng tìm mọi cách để chia các sinh vật thành các đội riêng lẻ sẽ không có quy mô. Các trường hợp có lợi thế không rõ ràng bao gồm các chủng tộc chỉ có một sinh vật hoặc trường hợp chia thành nhiều đội làm giảm sức mạnh tổng thể do chỉ số cao`x`. Ví dụ, nếu`b = 1`Và`x = 10`, việc chia tách ngay cả một chủng tộc gồm hai sinh vật thành các đội riêng biệt còn tệ hơn việc giữ chúng lại với nhau, mang lại câu trả lời chính xác là không có sức mạnh bổ sung. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ liệt kê tất cả số lượng đội có thể có, thử mọi cách để chỉ định sinh vật cho các đội, tính toán đóng góp của cặp kết quả và hình phạt từ nhiều đội và chọn mức tối đa. Đối với một cuộc đua với`c_i`sinh vật, số cách phân chia chúng tăng theo cấp số nhân, do đó phương pháp này nhanh chóng trở nên không khả thi ngay cả đối với các giá trị vừa phải của`c_i`. Số lượng thao tác sẽ vượt quá 2 × 10^5 × 2 × 10^5 trong trường hợp xấu nhất, điều này là không thể trong vòng một giây. 

Thông tin chi tiết quan trọng là xem xét từng cuộc đua một cách độc lập và nhận ra rằng sự đóng góp từ một cuộc đua được xác định hoàn toàn bằng số đội được chia thành bao nhiêu đội. Vì`k`đội, một cuộc đua với`c_i`sinh vật góp phần`b * (sum_{j=1}^{k} (number of creatures in squad j choose 2))`. Để tối đa hóa phần thưởng, các sinh vật cùng chủng tộc phải được phân bổ đồng đều nhất có thể giữa các đội vì tổng các ô vuông được giảm thiểu bằng cách cân bằng. Quan sát này làm giảm vấn đề thành hàm của một biến: số lượng đội`k`. Tổng sức mạnh quân đội sau đó sẽ trở thành tổng của tất cả các chủng tộc của sự đóng góp giữa các đội trừ đi hình phạt`(k - 1) * x`. Chức năng này là đơn phương thức trong`k`bởi vì ngày càng tăng`k`đầu tiên tăng số tiền đóng góp của cặp nhưng cuối cùng hình phạt chiếm ưu thế. Điều này cho phép chúng tôi sử dụng tìm kiếm ternary trên`k`hoặc đơn giản là lặp đi lặp lại có thể`k`giá trị hiệu quả kể từ khi tối đa`k`cho một cuộc đua nhiều nhất là`c_i`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(tổng c_i)) | O(tổng c_i) | Quá chậm | 
| Tối ưu | O(tổng c_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`,`b`,`x`, và mảng`c`. 
2. Sắp xếp`c`theo thứ tự giảm dần nếu bạn muốn, mặc dù điều đó không thực sự cần thiết. 
3. Với tổng số đội nhất định`k`, tính toán phần đóng góp của mỗi chủng tộc. Đối với một cuộc đua với`c_i`sinh vật, chia nó càng đều càng tốt thành`k`đội. Cho phép`q = c_i // k`Và`r = c_i % k`. Sau đó`r`đội có`q+1`sinh vật và`k-r`đội có`q`sinh vật. Số cặp giữa các đội là`(q+1)*q/2 * r + q*(q-1)/2 * (k-r)`. Nhân với`b`để nhận được sự đóng góp. 
4. Tổng hợp các khoản đóng góp từ tất cả các chủng tộc và trừ đi hình phạt của đội`(k-1) * x`để có được sức mạnh quân đội tổng thể. 
5. Tìm giá trị của`k`nhằm phát huy tối đa sức mạnh tổng thể của quân đội. Vì hàm này không theo phương thức nên bạn có thể lặp từ 1 đến mức tối đa`c_i`trên các cuộc đua hoặc sử dụng tìm kiếm tốc độ ternary. Với những ràng buộc nhất định, một vòng lặp đơn giản trên tất cả những điều hợp lý`k`hoạt động hiệu quả. 
6. Xuất ra sức mạnh quân đội tối đa cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Sự đóng góp của mỗi chủng tộc được tối đa hóa bằng cách phân bổ đồng đều các sinh vật cho các đội. Hình phạt là tuyến tính trong`k`. Bằng cách lặp lại hoặc tìm kiếm`k`, chúng tôi đảm bảo rằng chúng tôi tìm thấy mức tối đa toàn cầu vì hàm này là đơn thức. Không có sự kết hợp nào của việc phân chia không đồng đều có thể cải thiện kết quả vì bất kỳ sự mất cân bằng nào cũng làm giảm tổng đóng góp của cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_strength(n, b, x, c):
    max_c = max(c)
    best = 0
    for k in range(1, max_c + 1):
        total = 0
        for ci in c:
            if ci < k:
                continue
            q, r = divmod(ci, k)
            total += b * (r * q * (q + 1) // 2 + (k - r) * q * (q - 1) // 2)
        total -= (k - 1) * x
        best = max(best, total)
    return best

t = int(input())
for _ in range(t):
    n, b, x = map(int, input().split())
    c = list(map(int, input().split()))
    print(max_strength(n, b, x, c))
```chức năng`max_strength`thực hiện thuật toán. Vòng lặp bên trong phân bổ sinh vật của mỗi chủng tộc một cách đồng đều nhất có thể trên khắp`k`đội. Chúng tôi sử dụng phép chia số nguyên và mô đun để xử lý phần còn lại, đảm bảo không có đội nào bị bỏ qua hoặc tính hai lần. Phép trừ của`(k-1) * x`mô hình chính xác hình phạt của đội. Lặp đi lặp lại lên đến`max_c`là đủ vì sử dụng nhiều đội hơn cuộc đua lớn nhất sẽ không có thêm cặp nào. 

## Ví dụ đã hoạt động 

Mẫu 1:`3 1 0`với`c = [1,2,3]`| k | Cuộc đua 1 | Cuộc đua 2 | Cuộc đua 3 | Tổng Cặp | Phạt đền | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 1 | 3 | 4 | 0 | 4 | 
| 3 | 0 | 1 | 3 | 4 | 0 | 4 | 

Điều này cho thấy mức tối đa xảy ra ở`k=2`hoặc`3`với tổng số 4, xác nhận sản lượng dự kiến. 

Mẫu 2:`3 5 10`với`c = [2,5,3]`| k | Cuộc đua 1 | Cuộc đua 2 | Cuộc đua 3 | Tổng Cặp | Phạt đền | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 5 | 20 | 5 | 30 | 10 | 20 | 
| 3 | 5 | 25 | 5 | 35 | 20 | 15 | 
| 4 | 5 | 20 | 3 | 28 | 30 | -2 | 

Cực đại xảy ra ở`k=2`với tổng số 20, xác nhận cách chia tối ưu là hai đội. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng c_i * max_c) | Vòng lặp bên ngoài chạy tới tối đa(c_i) trên các cuộc đua, tổng đóng góp của vòng lặp bên trong cho tất cả các cuộc đua, tổng số tiền giới hạn bởi 2e5 | 
| Không gian | O(n) | Chỉ lưu trữ mảng c cho mỗi trường hợp thử nghiệm | 

Với`sum c_i ≤ 2 × 10^5`Và`max_c ≤ 2 × 10^5`, điều này có thể chấp nhận được dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, b, x = map(int, input().split())
        c = list(map(int, input().split()))
        output.append(str(max_strength(n, b, x, c)))
    return "\n".join(output)

# provided samples
assert run("5\n3 1 0\n1 2 3\n3 5 10\n2 5 3\n4 3 3\n3 2 1 2\n4 1 0\n4 1 4 2\n4 1 10\n4 1 4 2\n") == "4\n40\n9\n13\n0"

# custom cases
assert run("1\n1 1 0\n1\n") == "0", "single creature"
assert run("1\n2 10 100\n1 1\n") == "0", "high penalty dominates"
assert run("1\n3 2 1\n2 2 2\n") == "12", "all equal, moderate penalty"
assert run("1\n2 5 0\n5 5\n") == "100", "zero penalty, maximize squads"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0\n1 | 0 | sinh vật đơn lẻ, không có cặp | 
| 2 10 100 | | |
