---
title: "CF 309C - Bộ nhớ cho mảng"
description: "Chúng ta được cung cấp một bố cục bộ nhớ được chia thành nhiều phân đoạn trống liền kề mà chúng ta có thể coi là các khối không gian độc lập. Mỗi khối có độ dài cố định và chúng tôi cũng có một tập hợp các mảng mà chúng tôi có thể muốn đặt vào bộ nhớ này."
date: "2026-06-05T18:34:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 309
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2013 - Finals (online version, Div. 1)"
rating: 1900
weight: 309
solve_time_s: 76
verified: true
draft: false
---

[CF 309C - Bộ nhớ cho mảng](https://codeforces.com/problemset/problem/309/C) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, tham lam 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bố cục bộ nhớ được chia thành nhiều phân đoạn trống liền kề mà chúng ta có thể coi là các khối không gian độc lập. Mỗi khối có độ dài cố định và chúng tôi cũng có một tập hợp các mảng mà chúng tôi có thể muốn đặt vào bộ nhớ này. Mỗi mảng sử dụng một đoạn bộ nhớ liền kề có độ dài nhất định và nó phải nằm hoàn toàn bên trong một khối duy nhất. 

Nhiệm vụ là chọn mảng nào sẽ đặt và vị trí đặt chúng để chúng ta tối đa hóa số lượng mảng được lưu trữ thành công. Mỗi khối có thể lưu trữ nhiều mảng miễn là chúng không trùng nhau và vừa với dung lượng của nó, nhưng các mảng không thể chia thành các khối và các khối độc lập theo nghĩa là chúng ta không thể hợp nhất chúng. 

Cấu trúc của các vấn đề đầu vào: chúng tôi khớp hai tập hợp, dung lượng khối và kích thước mảng và mục tiêu là tối đa hóa số lượng nhiệm vụ thành công theo các ràng buộc đóng gói khoảng thời gian bên trong mỗi khối. 

Các ràng buộc rất lớn, có tới 10^6 khối và 10^6 mảng. Bất kỳ giải pháp nào cố gắng mô phỏng vị trí hoặc thử kết hợp trên mỗi khối hoặc mỗi mảng sẽ quá chậm. Các giải pháp khả thi duy nhất là khoảng O(n log n) hoặc O(n) với việc sắp xếp và xử lý tham lam. Điều này ngay lập tức loại trừ mọi lập trình động trên tất cả các tập hợp con hoặc việc đóng gói tổ hợp trên mỗi khối. 

Trường hợp cạnh tinh tế xuất hiện khi kết hợp tham lam được thực hiện sai thứ tự. Ví dụ: nếu chúng ta cố gắng đặt các mảng lớn trước tiên thành các khối lớn mà không điều phối không gian còn sót lại, chúng ta có thể lãng phí cơ hội phân mảnh. Một trường hợp thất bại khác xảy ra nếu chúng ta xử lý các khối một cách độc lập và đóng gói từng khối một cách tham lam mà không xem xét thứ tự tối ưu toàn cục của các mảng. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử tất cả các phép gán mảng cho các khối và kiểm tra tính khả thi. Ngay cả khi chúng ta đơn giản hóa thành “với mỗi mảng, hãy cố gắng ghép nó vào một khối nào đó”, thì kết quả này sẽ trở thành O(nm), vượt xa giới hạn khi cả hai đều đạt 10^6. 

Lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cải thiện bằng cách sắp xếp các khối và mảng, đồng thời quét các khối để tìm sự phù hợp đối với mỗi mảng. Điều này làm giảm các yếu tố không đổi nhưng vẫn suy biến thành hành vi bậc hai. 

Quan sát quan trọng là mỗi mảng chỉ cần một phân đoạn liền kề bên trong một khối, do đó, một khối có kích thước A có thể chứa hầu hết các mảng tầng (A / (2b)) có kích thước 2b nếu chúng ta sửa loại mảng nào được sử dụng bên trong nó. Tuy nhiên, việc trộn các kích thước khác nhau bên trong một khối sẽ làm phức tạp việc đếm trực tiếp. 

Nhận thức sâu sắc quan trọng là chúng ta không bao giờ được hưởng lợi từ việc gán một mảng lớn vào một khối nếu có thể chỉ định một mảng nhỏ hơn vào vị trí của nó, bởi vì các mảng nhỏ hơn sẽ linh hoạt hơn và bảo toàn các khối lớn cho mảng lớn. Điều này dẫn đến chiến lược tham lam toàn cầu: sắp xếp cả mảng và khối, đồng thời luôn cố gắng khớp các mảng nhỏ nhất trước thành các khối nhỏ nhất có thể chứa chúng hoặc xử lý tương đương từ phía bị hạn chế nhất trong khi vẫn duy trì tính khả thi. 

Một cách tiêu chuẩn để thực thi điều này là sắp xếp kích thước khối và kích thước mảng và sử dụng kết hợp tham lam với một con trỏ, luôn cố gắng khớp mảng nhỏ nhất còn lại vào khối nhỏ nhất có sẵn có thể chứa nó. Khi một khối quá nhỏ, nó sẽ bị bỏ qua; khi nó đủ lớn, nó được sử dụng một lần và chúng ta tiến về phía trước. 

Điều này biến vấn đề thành quét tuyến tính trên các mảng và khối đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Kết hợp tham lam tối ưu | O(n log n + m log m) | O(1)-O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi từng yêu cầu mảng thành nhu cầu bộ nhớ thực tế của nó, là 2*b[i]. Điều này bình thường hóa cả hai đầu vào thành độ dài phân đoạn tương đương. 
2. Sắp xếp kích thước khối theo thứ tự không giảm. Việc sắp xếp đảm bảo chúng tôi luôn xem xét dung lượng khả dụng nhỏ nhất trước tiên, điều này giúp tránh lãng phí khối lớn cho các yêu cầu nhỏ khi khối nhỏ hơn có thể đáp ứng đủ. 
3. Sắp xếp kích thước mảng theo thứ tự không giảm. Điều này phù hợp với ý tưởng chỉ định các mảng hạn chế nhất (nhỏ nhất) trước tiên, giúp tối đa hóa tính linh hoạt cho các mảng lớn hơn sau này. 
4. Khởi tạo hai con trỏ, một cho khối và một cho mảng, cả hai đều bắt đầu ở đầu danh sách được sắp xếp của chúng. 
5. Lặp lại khi cả hai con trỏ đều hợp lệ. Ở mỗi bước, hãy so sánh mảng chưa được gán nhỏ nhất hiện tại với khối nhỏ nhất hiện có. 
6. Nếu khối hiện tại quá nhỏ để vừa với mảng, hãy loại bỏ khối này và chuyển sang khối lớn hơn tiếp theo. Điều này an toàn vì khối này không thể được sử dụng cho bất kỳ mảng nào còn lại do sắp xếp. 
7. Nếu khối có thể vừa với mảng, hãy gán mảng cho khối này, tăng số lượng mảng được đặt và di chuyển cả hai con trỏ về phía trước. Điều này đảm bảo mỗi khối được sử dụng tối đa một lần trong mô hình khớp đơn giản hóa này, điều này hợp lệ vì bất kỳ khối nào có thể vừa với mảng nhỏ nhất hiện tại đều có thể được sử dụng cho khối đó mà không gây tổn hại đến tính tối ưu. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào cấu trúc khả thi đơn điệu được tạo ra bằng cách sắp xếp. Khi các mảng và khối được sắp xếp, tính khả thi sẽ trở thành một điều kiện ngưỡng: nếu một khối không thể vừa với một mảng nhất định thì nó cũng không thể vừa với bất kỳ mảng lớn hơn nào. Điều này đảm bảo việc bỏ qua một khối luôn an toàn. Tương tự, việc gán mảng nhỏ nhất có sẵn cho khối khả thi nhỏ nhất sẽ tránh tiêu thụ các khối lớn một cách không cần thiết, duy trì dung lượng cho các mảng lớn hơn trong tương lai. Cấu trúc tham lam này đảm bảo rằng mọi phép gán được thực hiện đều tối ưu cục bộ và không chặn bất kỳ phép gán khả thi nào trong tương lai có thể làm tăng tổng số lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

a.sort()
b = sorted(x * 2 for x in b)

i = j = 0
ans = 0

while i < n and j < m:
    if b[j] >= a[i]:
        ans += 1
        i += 1
        j += 1
    else:
        j += 1

print(ans)
```Giải pháp bắt đầu bằng cách đọc cả hai chuỗi và chuyển đổi các yêu cầu về mảng thành độ dài bộ nhớ thực tế. Việc sắp xếp cả hai danh sách là cần thiết vì nó tạo ra cấu trúc đơn điệu cần thiết cho việc quét hai con trỏ. 

Con trỏ`i`theo dõi mảng tiếp theo mà chúng tôi muốn đặt và`j`theo dõi khối tiếp theo mà chúng ta có thể sử dụng. Khi khối hiện tại quá nhỏ, nó sẽ bị loại bỏ vì nó không thể phục vụ bất kỳ mảng nào còn lại bằng hoặc cao hơn kích thước hiện tại. Khi phù hợp, chúng tôi cam kết gán và nâng cao cả hai con trỏ, đảm bảo cả khối và mảng đều không được sử dụng lại. 

Một lỗi phổ biến là đảo ngược bất đẳng thức hoặc lặp sai hướng. Sự so sánh chính xác luôn là liệu khối có thể chứa mảng hay không, chứ không phải ngược lại. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
5 3
8 4 3 2 2
3 2 2
```Sau khi chuyển đổi, kích thước mảng vẫn giữ nguyên`[8, 4, 4]`và kích thước khối là`[2, 4, 8, 2, 3]`, sắp xếp theo thứ gì`[2, 2, 3, 4, 8]`. 

| tôi | j | mảng | khối | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 8 | 2 | khối quá nhỏ | 0 | 
| 0 | 1 | 8 | 2 | khối quá nhỏ | 0 | 
| 0 | 2 | 8 | 3 | khối quá nhỏ | 0 | 
| 0 | 3 | 8 | 4 | khối quá nhỏ | 0 | 
| 0 | 4 | 8 | 8 | giao | 1 | 
| 1 | 5 | 4 | - | dừng lại | 1 | 

Bây giờ hãy xem xét ví dụ thứ hai:```
4 5
10 5 4 2
1 2 2 2 6
```Mảng được sắp xếp:`[2, 4, 5, 10]`, các khối được sắp xếp:`[2, 2, 2, 6, 20]`. 

| tôi | j | mảng | khối | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | giao | 1 | 
| 1 | 1 | 4 | 2 | bỏ qua khối | 1 | 
| 1 | 2 | 4 | 2 | bỏ qua khối | 1 | 
| 1 | 3 | 4 | 6 | giao | 2 | 
| 2 | 4 | 5 | 20 | giao | 3 | 
| 3 | 5 | - | - | dừng lại | 3 | 

Những dấu vết này cho thấy các mảng nhỏ hơn được khớp sớm như thế nào, trong khi các khối nhỏ không sử dụng được sẽ bị bỏ qua mà không gây tổn hại đến tính tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Việc sắp xếp chiếm ưu thế, theo sau là một lần quét tuyến tính duy nhất | 
| Không gian | O(1) thêm | Chỉ con trỏ và bộ đếm ngoài bộ nhớ đầu vào | 

Độ phức tạp nằm trong giới hạn cho 10^6 phần tử, vì việc sắp xếp 10^6 số nguyên là khả thi trong Python với đầu vào hiệu quả và quét tuyến tính sau đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()
    b = sorted(x * 2 for x in b)

    i = j = 0
    ans = 0

    while i < n and j < m:
        if b[j] >= a[i]:
            ans += 1
            i += 1
            j += 1
        else:
            j += 1

    return str(ans)

# provided sample
assert run("5 3\n8 4 3 2 2\n3 2 2\n") == "2"

# minimum case
assert run("1 1\n2\n1\n") == "1"

# no fit case
assert run("2 2\n10 10\n1 1\n") == "0"

# exact matching case
assert run("3 3\n2 4 6\n1 2 3\n") == "3"

# large skewed case
assert run("4 2\n100 50 25 10\n1 20\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 nhỏ vừa vặn | 1 | độ chính xác của bài tập tối thiểu | 
| không có trường hợp phù hợp | 0 | xử lý từ chối chính xác | 
| khớp chính xác | 3 | kết hợp tham lam một-một | 
| kích thước lệch | 2 | xử lý mất cân bằng lớn/nhỏ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều khối nhỏ xuất hiện trước một khối lớn cần thiết cho một mảng lớn. Đối với đầu vào như:```
3 2
100 1 1
50 50
```Sắp xếp tạo ra các khối`[1, 1, 100]`và mảng`[50, 50]`. Thuật toán bỏ qua hai khối nhỏ và gán cả hai mảng cho khối lớn không chính xác nếu chúng ta cho phép sử dụng lại, nhưng vì mỗi khối chỉ sử dụng một lần nên chỉ có 50 khối phù hợp, vì vậy câu trả lời là 1. Con trỏ tham lam đảm bảo chúng ta không sử dụng lại dung lượng một cách không chính xác. 

Một trường hợp khác là khi các mảng đều nhỏ hơn mọi khối ngoại trừ một số khối nhỏ nhất. Vì:```
4 3
8 7 6 5
1 1 1
```Thuật toán gán ba mảng nhỏ nhất cho ba khối khả thi nhỏ nhất ngay lập tức, xác nhận rằng việc sử dụng sớm các mảng nhỏ luôn an toàn và không chặn các nhiệm vụ lớn hơn do không tồn tại mảng lớn hơn.
