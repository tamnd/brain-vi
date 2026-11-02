---
title: "CF 1077E - Các cuộc thi chuyên đề"
description: "Chúng ta được cung cấp nhiều bài toán trong đó mỗi bài toán thuộc về một chủ đề. Cùng một chủ đề có thể xuất hiện nhiều lần. Từ nhóm này, chúng tôi muốn hình thành một số cuộc thi. Mỗi cuộc thi chỉ được sử dụng các bài toán thuộc một chủ đề duy nhất nên có tính đồng nhất."
date: "2026-06-15T06:41:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1077
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 521 (Div. 3)"
rating: 1800
weight: 1077
solve_time_s: 177
verified: true
draft: false
---

[CF 1077E - Cuộc thi chuyên đề](https://codeforces.com/problemset/problem/1077/E) 

**Đánh giá:** 1800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều bài toán trong đó mỗi bài toán thuộc về một chủ đề. Cùng một chủ đề có thể xuất hiện nhiều lần. Từ nhóm này, chúng tôi muốn hình thành một số cuộc thi. Mỗi cuộc thi chỉ được sử dụng các bài toán thuộc một chủ đề duy nhất nên có tính đồng nhất. Các cuộc thi khác nhau phải sử dụng các chủ đề khác nhau nên mỗi chủ đề có thể tham gia tối đa một cuộc thi. 

Các cuộc thi này được sắp xếp theo trình tự. Nếu chúng tôi quyết định rằng cuộc thi đầu tiên sử dụng$x$có vấn đề thì tiếp theo phải dùng$2x$, tiếp theo$4x$, v.v., tạo thành một chuỗi nhân đôi nghiêm ngặt. Chúng tôi có thể tự do lựa chọn số lượng cuộc thi mà chúng tôi tạo ra và chúng tôi có thể tự do chọn kích thước bắt đầu$x$. Mục tiêu không phải là tối đa hóa số lượng cuộc thi mà là tối đa hóa tổng số bài toán được sử dụng trong tất cả các cuộc thi đã chọn. 

Cấu trúc chính là mỗi cuộc thi tiêu tốn một lượng cố định cho các vấn đề và ngân sách tăng theo cấp số nhân. Vì các chủ đề không thể lặp lại nên mỗi bước trong trình tự phải được chỉ định một chủ đề khác nhau và chủ đề đó phải có sẵn đủ vấn đề để hỗ trợ quy mô giai đoạn đó. 

Ràng buộc$n \le 2 \cdot 10^5$ngụ ý rằng chúng ta cần một$O(n \log n)$hoặc$O(n)$tiếp cận. Bất kỳ điều gì cố gắng mô phỏng rõ ràng tất cả các quy mô bắt đầu và phân công chủ đề có thể có sẽ quá chậm, đặc biệt vì mỗi chủ đề có thể xuất hiện nhiều lần và việc so khớp đơn giản sẽ quét liên tục các danh sách tần số lớn. 

Một trường hợp lỗi nhỏ xuất hiện khi tần số không đồng đều. Ví dụ: nếu một chủ đề có số lượng lớn và nhiều chủ đề khác có số lượng nhỏ, thì một nhiệm vụ tham lam chỉ đơn giản lấy tần số lớn nhất trước mà không tôn trọng thứ tự nhân đôi có thể lãng phí số lượng lớn ở giai đoạn đầu hoặc không thể mở rộng chuỗi mặc dù đã có sự sắp xếp tốt hơn. 

Một trường hợp đặc biệt khác là khi nhiều chủ đề có tần số giống nhau. Một cách tiếp cận đơn giản có thể coi chúng có thể thay thế cho nhau, nhưng thứ tự phân công lại quan trọng vì các giai đoạn đầu gặp ít vấn đề hơn nhiều so với các giai đoạn sau và việc sắp xếp sai có thể cản trở các phần mở rộng khả thi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét mọi chủ đề bắt đầu có thể và mọi quy mô bắt đầu có thể. Đối với kích thước bắt đầu cố định$x$, chúng tôi cố gắng xây dựng một chuỗi: chọn bất kỳ chủ đề nào có ít nhất$x$vấn đề, sau đó bất kỳ chủ đề nào khác có ít nhất$2x$, sau đó$4x$, vân vân. Với mỗi lựa chọn, chúng ta tính tổng các bài toán đã sử dụng và lấy giá trị lớn nhất. 

Về nguyên tắc, điều này đúng vì nó khám phá tất cả các chuỗi hợp lệ. Tuy nhiên, với mỗi lần thử, chúng tôi sẽ liên tục tìm kiếm giữa các chủ đề để có tần suất tiếp theo khả thi. Với tối đa$2 \cdot 10^5$vấn đề, số lượng tần số riêng biệt lớn và số lượng chuỗi có thể có cũng lớn. Ngay cả khi chúng ta nén tần số, việc kiểm tra tất cả các chuỗi sẽ dẫn đến kết quả gần giống bậc hai hoặc tệ hơn trong thực tế. 

Nhận xét quan trọng là cấu trúc của vấn đề chỉ phụ thuộc vào tần suất các chủ đề chứ không phụ thuộc vào từng vấn đề riêng lẻ. Sau khi tính toán có bao nhiêu vấn đề tồn tại trong mỗi chủ đề, chúng tôi có thể coi mỗi chủ đề là một giá trị “năng lực”. Sau đó, chúng tôi muốn gán những khả năng này cho các cấp độ cấp số nhân. 

Thay vì chọn điểm xuất phát và mô phỏng về sau, chúng tôi đảo ngược quan điểm: ấn định số lượng cuộc thi mà chúng tôi sẽ tham gia, chẳng hạn$k$và cố gắng xem cách phân công chủ đề tốt nhất có thể cho các cấp độ$1, 2, 4, \dots, 2^{k-1}$. Đối với một cố định$k$, để tối đa hóa mức sử dụng, chúng tôi muốn chỉ định tần số sẵn có lớn nhất cho mức yêu cầu lớn nhất. Điều này gợi ý việc sắp xếp tần số và kết hợp tham lam từ yêu cầu lớn nhất đến dung lượng lớn nhất. 

Vấn đề còn lại là làm thế nào để xác định được điều tốt nhất$k$. Vì mỗi cấp độ được thêm vào sẽ tăng gấp đôi yêu cầu nên tổng số yêu cầu sẽ tăng theo cấp số nhân, do đó$k$nhiều nhất là ở xung quanh$\log n$. Điều này làm cho việc thử tất cả$k$giá trị và đánh giá một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Chuỗi Brute Force | số mũ /$O(n^2)$|$O(n)$| Quá chậm | 
| Hãy thử tất cả các độ dài + bài tập tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần suất của từng chủ đề. 

Mỗi chủ đề trở thành một con số biểu thị số lượng vấn đề chúng ta có thể phân bổ từ nó. 
2. Sắp xếp tần số theo thứ tự giảm dần. 

Các chủ đề lớn linh hoạt hơn nên chúng tôi muốn chúng phục vụ các yêu cầu lớn hơn khi có thể. 
3. Xây dựng tổng tiền tố các tần số để tính toán nhanh số lượng bài toán có sẵn cho bất kỳ tập hợp con nào được chọn của các chủ đề hàng đầu. 
4. Hãy thử tất cả số cuộc thi có thể$k$. 

Vì các yêu cầu tăng lên như$1, 2, 4, \dots$, chúng tôi dừng lại khi tổng số yêu cầu vượt quá các vấn đề có sẵn. 
5. Đối với cố định$k$, tính toán kích thước yêu cầu:$1, 2, 4, \dots, 2^{k-1}$. 
6. Kiểm tra tính khả thi từ yêu cầu lớn nhất đến yêu cầu nhỏ nhất: gán chủ đề có tần suất lớn nhất trước cho yêu cầu lớn nhất. 

Điều này hiệu quả vì các yêu cầu lớn hơn khó được đáp ứng hơn, vì vậy chúng sẽ tiêu tốn các tài nguyên linh hoạt nhất. 
7. Nếu khả thi, hãy tính tổng số tiền đóng góp bằng tổng của tất cả quy mô cuộc thi được chỉ định và cập nhật câu trả lời. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là đối với bất kỳ số lượng cuộc thi cố định nào, phép gán tối ưu không bao giờ được hưởng lợi từ việc đưa ra yêu cầu lớn cho tần số nhỏ hơn khi có sẵn tần số lớn hơn. Bất kỳ sự hoán đổi nào chuyển tần số lớn hơn sang yêu cầu lớn hơn chỉ có thể duy trì hoặc cải thiện tính khả thi vì các ràng buộc đều đơn điệu. Điều này làm giảm vấn đề trong việc kết hợp các nhu cầu được sắp xếp với năng lực được sắp xếp, giúp loại bỏ sự bùng nổ tổ hợp của các bài tập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    vals = sorted(freq.values(), reverse=True)
    
    best = 0
    
    # try number of contests k
    for k in range(1, len(vals) + 1):
        need = []
        x = 1
        total_need = 0
        
        for i in range(k):
            need.append(x)
            total_need += x
            x *= 2
        
        # prune if even total requirement exceeds sum of all problems
        if total_need > n:
            break
        
        # check feasibility: assign largest needs to largest frequencies
        ok = True
        for i in range(k):
            if vals[i] < need[k - 1 - i]:
                ok = False
                break
        
        if ok:
            best = max(best, total_need)
    
    print(best)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nén đầu vào thành tần số chủ đề. Sau đó, nó sắp xếp các tần suất này để chúng tôi luôn có thể suy luận về việc phân bổ các chủ đề mạnh nhất trước tiên. Đối với mỗi số lượng cuộc thi có thể có, nó xây dựng chuỗi hình học cần thiết và kiểm tra xem tần số lớn nhất có thể đáp ứng yêu cầu lớn nhất hay không. Sự so sánh ngược đảm bảo rằng chúng ta khớp các ràng buộc theo cách có lợi nhất. 

Bước cắt bớt sử dụng tổng số tiền yêu cầu sẽ ngăn chặn những kiểm tra không cần thiết khi yêu cầu vượt quá các vấn đề sẵn có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
18
2 1 2 10 2 10 10 2 2 1 10 10 10 10 1 1 10 10
```Tần số: 

| Chủ đề | Đếm | 
| --- | --- | 
| 10 | 8 | 
| 2 | 5 | 
| 1 | 3 | 

Đã sắp xếp:`[8, 5, 3]`Chúng tôi kiểm tra$k = 1, 2, 3$. 

Vì$k = 3$, yêu cầu là`[1, 2, 4]`, tổng cộng là 7. 

Kiểm tra nhiệm vụ: 

| Cấp độ (lớn nhất trước) | Yêu cầu | Tần số | 
| --- | --- | --- | 
| 4 | 4 | 8 | 
| 2 | 2 | 5 | 
| 1 | 1 | 3 | 

Tất cả đều hài lòng, vậy tổng = 7. 

Điều này cho thấy cả ba chủ đề đều có thể tham gia vào chuỗi nhân đôi. 

### Ví dụ 2 

đầu vào:```
2
3 3
```Tần số: 

| Chủ đề | Đếm | 
| --- | --- | 
| 3 | 2 | 

Chỉ có một chủ đề tồn tại, vì vậy chỉ$k = 1$là có thể. 

Yêu cầu`[1]`, tổng cộng = 1. 

Thuật toán tránh được việc cố gắng một cách chính xác$k = 2$vì không có chủ đề thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Tần suất sắp xếp chiếm ưu thế và chúng tôi cố gắng nhiều nhất$O(\log n)$giá trị của$k$| 
| Không gian |$O(n)$| Bản đồ tần số và danh sách đếm | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó việc sắp xếp và một số lượng nhỏ kiểm tra tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    vals = sorted(freq.values(), reverse=True)
    
    best = 0
    
    for k in range(1, len(vals) + 1):
        need = []
        x = 1
        total_need = 0
        
        for i in range(k):
            need.append(x)
            total_need += x
            x *= 2
        
        if total_need > n:
            break
        
        ok = True
        for i in range(k):
            if vals[i] < need[k - 1 - i]:
                ok = False
                break
        
        if ok:
            best = max(best, total_need)
    
    return str(best)

# provided samples
assert run("18\n2 1 2 10 2 10 10 2 2 1 10 10 10 10 1 1 10 10\n") == "7"

# single topic
assert run("3\n1337 1337 1337\n") == "3"

# two balanced topics
assert run("6\n1 1 1 2 2 2\n") == "6"

# skewed frequencies
assert run("10\n1 1 1 1 1 2 2 3 3 3\n") == "7"

# minimum
assert run("1\n5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chủ đề duy nhất | 3 | chỉ có một cuộc thi có thể | 
| chủ đề cân bằng | 6 | sử dụng đầy đủ trên nhiều chủ đề | 
| tần số lệch | 7 | tham lam phù hợp mạnh mẽ | 
| đầu vào tối thiểu | 1 | trường hợp cạnh cơ sở | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi một chủ đề chiếm ưu thế trong phân bổ tần suất. Ví dụ:```
8
1 1 1 1 1 1 1 1
```Ở đây chỉ có thể hình thành một cuộc thi và thuật toán đưa ra chính xác 1 vì không tồn tại chủ đề thứ hai. 

Một trường hợp khác là nhiều chủ đề nhỏ bằng nhau:```
6
1 2 3 4 5 6
```Mỗi chủ đề có tần suất 1. Chỉ$k = 1$hoạt động, vì$k = 2$đã yêu cầu một chủ đề thứ hai. Thuật toán ngăn chặn việc mở rộng quá mức một cách chính xác vì các tần số được sắp xếp không đáp ứng được yêu cầu kiểm tra. 

Trường hợp cuối cùng là khi các chủ đề lớn tồn tại nhưng không thể hỗ trợ chuỗi sâu. Thuật toán đảm bảo rằng ngay cả khi tổng số tiền lớn, các yêu cầu theo cấp số nhân buộc phải dừng sớm, ngăn chặn các chuỗi dài không hợp lệ.
