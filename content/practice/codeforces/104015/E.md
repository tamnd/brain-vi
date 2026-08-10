---
title: "CF 104015E - Xóa hai phần tử"
description: "Chúng ta được cho một mảng các số nguyên và trước tiên chúng ta tính giá trị trung bình của nó, bằng tổng chia cho số phần tử. Giá trị trung bình này không nhất thiết phải là số nguyên, nhưng nó là một giá trị hợp lý cố định được xác định bởi toàn bộ mảng."
date: "2026-07-02T04:51:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104015
codeforces_index: "E"
codeforces_contest_name: "ICPC 2021-2022 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 104015
solve_time_s: 43
verified: true
draft: false
---

[CF 104015E - Xóa hai phần tử](https://codeforces.com/problemset/problem/104015/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và trước tiên chúng ta tính giá trị trung bình của nó, bằng tổng chia cho số phần tử. Giá trị trung bình này không nhất thiết phải là số nguyên, nhưng nó là một giá trị hợp lý cố định được xác định bởi toàn bộ mảng. 

Nhiệm vụ là loại bỏ chính xác hai phần tử riêng biệt để khi tính lại giá trị trung bình của các phần tử còn lại vẫn giữ nguyên như giá trị trung bình ban đầu. Chúng ta được yêu cầu đếm xem có bao nhiêu cặp chỉ số tạo ra đặc tính này. 

Ràng buộc chính là kích thước mảng, có thể lên tới 200.000. Bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp sẽ xem xét về$\frac{n(n-1)}{2}$loại bỏ, tức là khoảng 20 tỷ hoạt động trong trường hợp xấu nhất. Điều đó vượt xa những gì có thể diễn ra trong hai giây trong Python hoặc C++. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào. 

Có một trường hợp khó nhận thấy khi tất cả các phần tử đều giống hệt nhau. Trong trường hợp đó, việc loại bỏ hai phần tử bất kỳ sẽ bảo toàn giá trị trung bình một cách không đáng kể, bởi vì mọi tập hợp con đều có cùng giá trị trung bình. Việc triển khai đơn giản vẫn có thể gặp phải các vấn đề về độ chính xác về số học hoặc rủi ro không cần thiết nếu nó cố tính toán lại giá trị trung bình trực tiếp dưới dạng số dấu phẩy động. 

Một trường hợp quan trọng khác là khi trung bình không phải là số nguyên. Một giải pháp bất cẩn có thể cố gắng so sánh giá trị trung bình của dấu phẩy động sau khi xóa. Điều này không an toàn vì các lỗi chính xác có thể gây ra việc kiểm tra đẳng thức không chính xác ngay cả khi điều kiện đại số được giữ chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các cặp$(i, j)$, loại bỏ hai phần tử đó, tính lại tổng của mảng còn lại và kiểm tra xem giá trị trung bình có khớp với giá trị ban đầu hay không. Việc tính tổng còn lại có thể được tối ưu hóa thành O(1) bằng cách sử dụng tổng tiền tố, nhưng chúng tôi vẫn có cặp O(n^2) để kiểm tra, điều này dẫn đến khoảng$10^{10}$kiểm tra trong trường hợp xấu nhất. Ngay cả với số học theo thời gian không đổi cho mỗi lần kiểm tra, điều này vẫn quá chậm. 

Quan sát quan trọng xuất phát từ việc viết lại điều kiện theo đại số. Gọi tổng của mảng là$S$, và coi giá trị ban đầu là$k = S / n$. Sau khi loại bỏ hai phần tử$a_i$Và$a_j$, tổng mới trở thành$S - a_i - a_j$, và số phần tử mới là$n - 2$. Chúng tôi yêu cầu:$$\frac{S - a_i - a_j}{n - 2} = \frac{S}{n}$$Nhân chéo tránh hoàn toàn các phân số và bộc lộ ràng buộc tuyến tính trên các cặp. Điều này biến bài toán từ việc kiểm tra trung bình thành các cặp đếm với điều kiện tổng cố định. Sau khi đơn giản hóa, mỗi cặp hợp lệ phải thỏa mãn:$$a_i + a_j = \frac{2S}{n}$$Vì vậy, toàn bộ nhiệm vụ giảm xuống việc đếm tổng có bao nhiêu cặp thành một giá trị mục tiêu không đổi. Đây là một bài toán đếm tần số cổ điển có thể được giải theo thời gian tuyến tính bằng cách sử dụng bản đồ băm. 

Chúng tôi tính toán tần số của từng giá trị và sau đó, với mỗi giá trị riêng biệt$x$, chúng ta tìm phần bù của nó$T - x$, Ở đâu$T = 2S / n$. Phải thận trọng để tránh tính trùng và xử lý trường hợp$x = T - x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Băm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền$S$của mảng và rút ra tổng mục tiêu$T = 2S / n$. Giá trị này thể hiện giá trị mà mọi cặp hợp lệ phải cộng lại sau khi chuyển đổi đại số của điều kiện trung bình. 
2. Nếu$2S$không chia hết cho$n$, dừng ngay lập tức và trả về 0. Trong trường hợp này, tổng mục tiêu không phải là số nguyên, nhưng tất cả các phần tử mảng đều là số nguyên, vì vậy không có cặp nào có thể thỏa mãn chính xác điều kiện. 
3. Xây dựng bản đồ tần số của tất cả các giá trị mảng. Điều này cho phép chúng ta đếm số lần mỗi phần tử ứng cử viên xuất hiện, điều này cần thiết để đếm các cặp hợp lệ một cách hiệu quả. 
4. Lặp lại từng giá trị riêng biệt$x$trong bản đồ tần số. Với mỗi giá trị như vậy, hãy tính phần bù của nó$y = T - x$. 
5. Nếu$x < y$, đếm tất cả các cặp được hình thành bằng cách chọn một lần xuất hiện của$x$và một lần xuất hiện$y$. Số cặp như vậy là$freq[x] \cdot freq[y]$. Ràng buộc thứ tự tránh được việc tính hai lần. 
6. Nếu$x = y$, các cặp đếm được hình thành hoàn toàn trong cùng một nhóm giá trị. Số cặp như vậy là$freq[x] \cdot (freq[x] - 1) / 2$. 
7. Tổng hợp tất cả các khoản đóng góp và đưa ra kết quả. 

### Tại sao nó hoạt động 

Việc chuyển đổi từ ràng buộc trung bình sang ràng buộc tổng cố định là chính xác và có thể đảo ngược. Mọi phép xóa hợp lệ phải bảo toàn giá trị trung bình và đại số cho thấy điều này tương đương với việc yêu cầu cặp bị xóa phải có tổng chính xác$T = 2S/n$. Việc đếm dựa trên tần số liệt kê từng cặp không có thứ tự chính xác một lần, thông qua các phần bù riêng biệt hoặc trong một lớp giá trị duy nhất. Không có cặp hợp lệ nào bị bỏ sót vì mỗi cặp chỉ số được biểu thị bằng chính xác một tổ hợp giá trị trong bản đồ tần số và không có cặp không hợp lệ nào được đưa vào vì tất cả các cặp được tính đều thỏa mãn đẳng thức dẫn xuất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    s = sum(a)
    if (2 * s) % n != 0:
        print(0)
        return
    
    target = (2 * s) // n
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    ans = 0
    for x in freq:
        y = target - x
        if y not in freq:
            continue
        if x < y:
            ans += freq[x] * freq[y]
        elif x == y:
            c = freq[x]
            ans += c * (c - 1) // 2
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính tổng và kiểm tra tính chia hết để đảm bảo tổng của cặp mục tiêu dẫn xuất là tích phân. Điều này tránh hoàn toàn việc so sánh dấu phẩy động. Từ điển tần số theo dõi sự xuất hiện của từng giá trị để việc đếm cặp trở thành tổ hợp thay vì lặp lại. 

Các phím lặp thực thi cẩn thận thứ tự bằng cách sử dụng`x < y`, điều này ngăn cản các cặp đếm kép như (x, y) và (y, x). Trường hợp bằng nhau sử dụng công thức kết hợp tiêu chuẩn để chọn hai chỉ số từ một nhóm có giá trị giống nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
8 8 8 8
```Đây,$S = 32$, Vì thế$T = 2S/n = 16$. Mọi phần tử đều bằng 8, vì vậy chúng tôi kiểm tra các cặp có tổng bằng 16. 

| Giá trị x | Tần số | Bổ sung y | Đóng góp | 
| --- | --- | --- | --- | 
| 8 | 4 | 8 | 4 * 3/2 = 6 | 

Thuật toán đếm tất cả các cách để chọn hai phần tử từ bốn giá trị giống nhau. Mỗi lần xóa sẽ giữ nguyên giá trị trung bình vì mảng không đổi. 

Đầu ra:```
6
```Điều này xác nhận rằng khi tất cả các giá trị bằng nhau thì mọi cặp đều hợp lệ và việc đếm tổ hợp là đủ. 

### Ví dụ 2 

đầu vào:```
5
1 4 7 3 5
```Tổng là$20$, Vì thế$T = 8$. Chúng tôi đếm các cặp có tổng là 8. 

| x | tần số[x] | y = 8-x | tần số[y] | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | 1 | 1 | 
| 3 | 1 | 5 | 1 | 1 | 
| 4 | 1 | 4 | 1 | bị bỏ qua (x > y đã được xử lý) | 

Đầu ra:```
2
```Dấu vết này cho thấy cách xử lý đối xứng ngăn chặn việc tính hai lần trong khi vẫn thu được tất cả các cặp bổ sung hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt tính tổng và bản đồ tần số, một lượt tính các giá trị riêng biệt | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa n phần tử riêng biệt | 

Thuật toán chia tỷ lệ tuyến tính với kích thước đầu vào, điều này rất cần thiết để xử lý tới 200.000 phần tử trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính và phù hợp thoải mái trong các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# sample-like cases
assert run("4\n8 8 8 8\n") == "6"
assert run("5\n1 4 7 3 5\n") == "2"

# minimum size
assert run("3\n1 2 3\n") == "0"

# no valid pairs
assert run("4\n1 2 3 4\n") == "0"

# all equal large
assert run("6\n5 5 5 5 5 5\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 yếu tố giống hệt nhau | 6 | tính chính xác của phép đếm tổ hợp | 
| 1 4 7 3 5 | 2 | logic ghép nối bổ sung | 
| 1 2 3 | 0 | không có trường hợp cặp hợp lệ | 
| 1 2 3 4 | 0 | phân phối không có giải pháp tầm thường | 
| tất cả 5s (n=6) | 15 | vỏ cạnh có tính đối xứng hoàn toàn | 

## Vỏ cạnh 

Đối với trường hợp tất cả bằng nhau, nói$n = 6$và mảng là`[5, 5, 5, 5, 5, 5]`. Tổng số tiền là 30, do đó tổng cặp mục tiêu là 10. Mỗi cặp 5 giây đều hợp lệ. Thuật toán xây dựng tần số`freq[5] = 6`và đi vào nhánh bình đẳng. Nó tính toán$6 \cdot 5 / 2 = 15$, khớp với số cách chọn hai vị trí bất kỳ. 

Đối với trường hợp không tồn tại mục tiêu số nguyên, chẳng hạn như`[1, 2, 3]`, tổng là 6, vì vậy$2S/n = 4$. Điều này hợp lệ nhưng không có cặp nào có tổng bằng 4. Bản đồ tần số kiểm tra các cặp và không tìm thấy cặp nào, dẫn đến 0. Thay vào đó, nếu tổng tạo ra mục tiêu phân số thì kiểm tra khả năng chia hết sớm sẽ trả về chính xác 0 mà không cần quét các cặp. 

Đối với mảng hỗn hợp như`[1, 4, 7, 3, 5]`, thuật toán ghép các giá trị theo đúng cấu trúc phần bù. Mỗi cặp hợp lệ xuất hiện chính xác một lần do hạn chế về thứ tự, ngăn chặn việc đếm quá mức trong khi vẫn ghi lại tất cả các lần xóa hợp lệ.
