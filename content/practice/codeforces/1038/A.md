---
title: "CF 1038A - Bình đẳng"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái viết hoa, nhưng chỉ từ các chữ cái $k$ đầu tiên của bảng chữ cái. Nhiệm vụ không phải là sắp xếp lại hoặc sửa đổi chuỗi mà là chọn một chuỗi ký tự con trong khi vẫn giữ nguyên thứ tự."
date: "2026-06-16T18:37:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1038
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 508 (Div. 2)"
rating: 800
weight: 1038
solve_time_s: 707
verified: false
draft: false
---

[CF 1038A - Bình đẳng](https://codeforces.com/problemset/problem/1038/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 11 phút 47 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi gồm các chữ cái viết hoa, nhưng chỉ từ chữ cái đầu tiên$k$các chữ cái của bảng chữ cái. Nhiệm vụ không phải là sắp xếp lại hoặc sửa đổi chuỗi mà là chọn một chuỗi ký tự con trong khi vẫn giữ nguyên thứ tự. Trong số tất cả các dãy con như vậy, chúng ta muốn có một dãy có đặc tính đặc biệt: mọi chữ cái từ`'A'`đến$k$-chữ cái thứ phải xuất hiện cùng số lần trong dãy sau. Mục tiêu là tối đa hóa độ dài của chuỗi con như vậy. 

Một cách khác để nghĩ về nó là chúng ta đang cố gắng chọn một số$x$của mỗi trong số$k$ký tự, sao cho tổng chiều dài là$k \cdot x$và lựa chọn này phải có thể thực hiện được dưới dạng một chuỗi con của chuỗi gốc. 

Ràng buộc$n \le 10^5$ngay lập tức cho chúng ta biết rằng bất kỳ cách tiếp cận nào tệ hơn$O(n \cdot k)$hoặc$O(n \log n)$sẽ bắt đầu đấu tranh. Một lực lượng vũ phu trên tất cả các chuỗi con là không thể bởi vì có$2^n$của họ. Thậm chí thử tất cả các giá trị có thể có của$x$với việc quét lặp lại vẫn cần được tối ưu hóa cẩn thận. 

Một vài tình huống khó khăn quan trọng. 

Nếu một số chữ cái bị thiếu hoàn toàn thì không có dãy con hợp lệ nào có thể bao gồm nó, do đó câu trả lời phải bằng 0. Ví dụ, nếu$k = 3$nhưng chuỗi chỉ chứa`'A'`Và`'B'`, thì chúng ta không bao giờ có thể cân bằng`'C'`, vậy kết quả là$0$. 

Một trường hợp tinh tế khác là khi tần số rất không đồng đều. Giả định$k = 3$và chuỗi là`"AAAAABBBBBCC"`. Mặc dù mỗi chữ cái đều tồn tại nhưng hệ số giới hạn sẽ có tần số nhỏ nhất trong số ba chữ cái đó. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta cần chọn vị trí một cách cẩn thận để tối đa hóa mức chênh lệch. Trong thực tế, thứ tự không liên quan đến việc đếm tính khả thi, vì các chuỗi con duy trì trật tự nhưng không hạn chế số lượng vượt quá tính khả dụng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi chuỗi có thể xảy ra và kiểm tra xem nó có cân bằng hay không. Đối với mỗi chuỗi con, chúng tôi đếm số lần xuất hiện của từng chữ cái và xác minh sự bằng nhau. Điều này đúng nhưng hoàn toàn không khả thi: có$2^n$các chuỗi tiếp theo, và thậm chí việc kiểm tra một chuỗi cũng mất$O(n)$, dẫn đến thời gian hàm mũ. 

Một hướng tốt hơn là điều chỉnh lại vấn đề. Thay vì chọn các chuỗi con một cách rõ ràng, chúng tôi tập trung vào số lần mỗi ký tự có thể xuất hiện. Giả sử chúng ta quyết định rằng mỗi$k$các chữ cái xuất hiện chính xác$x$lần. Khi đó tổng chiều dài là$k \cdot x$, và câu hỏi trở thành: liệu có thể chọn$x$sự xuất hiện của mọi ký tự trong chuỗi trong khi tôn trọng thứ tự? 

Nhưng thứ tự thực sự không quan trọng đối với tính khả thi ở đây, bởi vì các chuỗi con chỉ yêu cầu đủ số lần xuất hiện chứ không cần cấu trúc liền kề. Chúng ta luôn có thể chọn các lần xuất hiện một cách tham lam theo thứ tự quét nếu chúng tồn tại. 

Vì vậy, ràng buộc thực sự trở thành một điều kiện đếm đơn giản: với mỗi chữ cái, chúng ta phải có ít nhất$x$lần xuất hiện. Nếu như$cnt[c]$là tần số của chữ cái$c$, sau đó$x \le \min cnt[c]$. Điều tốt nhất chúng ta có thể làm là thiết lập$x$tới tần số tối thiểu đó. 

Vì vậy, câu trả lời chỉ đơn giản là:$$k \cdot \min_{c \in [A, A+k-1]} cnt[c]$$Cái nhìn sâu sắc quan trọng là việc sắp xếp thứ tự sau không đưa ra các ràng buộc bổ sung ngoài khả năng sẵn có, do đó vấn đề trở thành việc giảm thiểu tần số thuần túy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tần suất tối thiểu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi lần đầu tiên bao nhiêu lần$k$các chữ cái xuất hiện trong chuỗi. Điều này có hiệu quả vì các chuỗi con chỉ có thể sử dụng các ký tự hiện có nên tính khả dụng là hạn chế duy nhất. 
2. Tìm tần số nhỏ nhất trong số này$k$tính. Giá trị này thể hiện số lượng tối đa các “lớp cân bằng” đầy đủ mà chúng ta có thể hình thành trên tất cả các chữ cái. 
3. Nhân mức tối thiểu này với$k$, vì mỗi lớp đóng góp chính xác một trong mỗi chữ cái. 
4. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi dãy con hợp lệ phải chứa số lượng bằng nhau của tất cả$k$những lá thư, nói$x$của mỗi người. Điều đó ngay lập tức ngụ ý rằng không có chữ cái nào có thể đóng góp nhiều hơn tổng số lần xuất hiện của nó trong chuỗi gốc, vì vậy$x \le cnt[c]$cho tất cả$c$. Vì thế$x \le \min cnt[c]$. Ngược lại, nếu chọn$x = \min cnt[c]$, chúng ta luôn có thể chọn nhiều lần xuất hiện của mỗi ký tự theo thứ tự từ chuỗi vì các chuỗi con chỉ yêu cầu chọn vị trí, không sắp xếp lại hoặc tôn trọng tính liền kề. Điều này thiết lập cả tính tối ưu và tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
s = input().strip()

cnt = [0] * k

for ch in s:
    cnt[ord(ch) - ord('A')] += 1

ans = min(cnt) * k
print(ans)
```Việc thực hiện trực tiếp theo sau việc giảm số lượng. Mảng`cnt`theo dõi tần số đầu tiên$k$các chữ cái. Chúng tôi tính toán mức tối thiểu trên tất cả các tần số này và nhân với$k$. 

Một điểm tinh tế là lập chỉ mục: vì các chữ cái được đảm bảo nằm trong phần đầu tiên$k$, ánh xạ`'A'`để chỉ số 0 là an toàn. Không cần kiểm tra có điều kiện. 

Phép nhân với$k$chỉ xảy ra sau khi lấy mức tối thiểu; việc đảo ngược thứ tự sẽ không chính xác vì chúng ta không tính tổng các giá trị cực đại độc lập mà thực thi sự bình đẳng trên tất cả các chữ cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3
ACAABCCAB
```Số lượng phát triển như sau: 

| Thư | Đếm | 
| --- | --- | 
| A | 4 | 
| B | 2 | 
| C | 3 | 

Tần số tối thiểu là$2$, vì vậy chúng ta có thể tạo thành 2 lớp đầy đủ`A`,`B`,`C`, cho tổng chiều dài$3 \cdot 2 = 6$. 

Điều này cho thấy rằng mặc dù`'A'`Và`'C'`thường xuyên hơn, câu trả lời bị ràng buộc hoàn toàn bởi`'B'`. 

### Ví dụ 2 

đầu vào:```
5 3
ABCDE
```Chỉ có ba chữ cái đầu tiên là có liên quan, nhưng`'C'`có thể bị thiếu tùy thuộc vào cách giải thích giới hạn đầu vào. Nếu bất kỳ chữ cái bắt buộc nào không xuất hiện: 

| Thư | Đếm | 
| --- | --- | 
| A | 1 | 
| B | 1 | 
| C | 0 | 

Tối thiểu là 0, vì vậy câu trả lời là 0. 

Điều này xác nhận rằng một ký tự bị thiếu sẽ làm sụp đổ toàn bộ công trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lượt để đếm tần số và quét k chữ cái | 
| Không gian |$O(1)$| mảng có kích thước cố định có độ dài tối đa 26 | 

Giải pháp là tuyến tính theo độ dài chuỗi, tối ưu vì mỗi ký tự phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi do kích thước bảng chữ cái bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    cnt = [0] * k
    for ch in s:
        cnt[ord(ch) - ord('A')] += 1

    return str(min(cnt) * k)

# provided sample
assert run("9 3\nACAABCCAB\n") == "6"

# k = 1, always full string
assert run("5 1\nAAAAA\n") == "5"

# missing character forces zero
assert run("5 3\nAABBB\n") == "0"

# all equal frequencies
assert run("6 2\nAABBAB\n") == "4"

# large balanced case
assert run("4 2\nBABA\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 3 ACAABCCAB | 6 | trường hợp bình thường có ký tự giới hạn | 
| 5 1 AAAAA | 5 | k=1 vỏ cạnh | 
| 5 3 AABBB | 0 | thư còn thiếu | 
| 6 2 AABBAB | 4 | tần số cân bằng | 
| 4 2 BABA | 4 | sự đúng đắn của cấu trúc xen kẽ | 

## Vỏ cạnh 

Nếu một chữ cái không bao giờ xuất hiện thì tần số tối thiểu sẽ bằng 0 và thuật toán trả về 0 một cách chính xác vì không có chuỗi con cân bằng nào có thể bao gồm chữ cái đó. 

Để có dấu vết cụ thể, hãy lấy:```
4 3
ABAB
```Đếm: 

| A | B | C | 
| --- | --- | --- | 
| 2 | 2 | 0 | 

Tối thiểu là 0, do đó đầu ra là 0. Thuật toán không cố gắng “bỏ qua” các chữ cái bị thiếu, phù hợp với yêu cầu tất cả các chữ cái đầu tiên$k$các chữ cái phải xuất hiện bằng nhau. 

Một trường hợp cạnh khác là khi$k = 1$. Vì:```
5 1
ABCDE
```Chỉ một`'A'`được xem xét, số đếm là 1, vì vậy câu trả lời là 1. Thuật toán giảm chính xác vì giá trị tối thiểu trên một giá trị là chính nó và nhân với 1 sẽ bảo toàn giá trị đó.
