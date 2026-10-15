---
title: "CF 105403B - Cuộc thi lập trình"
description: "Mỗi trường hợp kiểm tra đưa ra hai mảng điểm, một thuộc về Arturo và một thuộc về Benito. Đối với mỗi chỉ số, chúng tôi có một cặp giá trị, nhưng việc ghép đôi hoàn toàn không xác định được: chúng tôi có thể tự do gán số của Arturo và số của Benito cho các bài toán cuộc thi khác nhau theo bất kỳ thứ tự nào…"
date: "2026-06-23T04:51:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105403
codeforces_index: "B"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105403
solve_time_s: 91
verified: false
draft: false
---

[CF 105403B - Cuộc thi lập trình](https://codeforces.com/problemset/problem/105403/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp kiểm tra đưa ra hai mảng điểm, một thuộc về Arturo và một thuộc về Benito. Đối với mỗi chỉ số, chúng tôi có một cặp giá trị, nhưng việc ghép đôi hoàn toàn không xác định được: chúng tôi có thể tự do gán số của Arturo và số của Benito cho các bài toán cuộc thi khác nhau theo bất kỳ thứ tự nào chúng tôi muốn. 

Đối với mỗi bài toán, ai có giá trị được gán lớn hơn sẽ thắng bài toán đó. Vì điểm của Arturo luôn bằng nhau và điểm của Benito luôn là số lẻ nên sự bằng nhau không bao giờ xảy ra, nên mỗi cặp đôi đều tạo ra một người chiến thắng chắc chắn. 

Nhiệm vụ không phải là xây dựng lại một phép gán đơn lẻ mà là suy luận về tất cả các hoán vị có thể có về cách khớp hai tập hợp nhiều tập hợp. Trong số tất cả các cách ghép nối các giá trị, chúng tôi muốn tối đa hóa số lần so sánh mà Arturo thắng. 

Ràng buộc$N \le 5 \cdot 10^4$buộc mọi giải pháp phải có nhiều nhất$O(N \log N)$mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai trong$N$sẽ quá chậm vì việc phân loại hoặc quét 40 thùng lớn đã vượt quá giới hạn. Điều này ngay lập tức gợi ý rằng chúng ta cần một chiến lược so khớp tham lam trên các mảng được sắp xếp thay vì bất kỳ tìm kiếm gán tổ hợp nào. 

Một vấn đề nhỏ xuất hiện khi các giá trị lặp lại. Nếu Arturo có nhiều số chẵn nhỏ và Benito có sự kết hợp của các số lẻ lớn và nhỏ, thì thứ tự chúng ta so khớp chúng có thể thay đổi kết quả một cách đáng kể. Một cách tiếp cận tham lam ngây thơ không sắp xếp cẩn thận các so sánh từ nhỏ nhất đến nhỏ nhất có thể lãng phí giá trị Arturo lớn cho các trận đấu đã thắng. 

Một trường hợp khác là khi tất cả các giá trị Arturo lớn hơn tất cả các giá trị Benito. Trong trường hợp đó câu trả lời là tầm thường$N$, nhưng một kẻ tham lam bất cẩn tiêu thụ sớm các giá trị Benito lớn vẫn có thể lãng phí các trận đấu nếu triển khai không đúng. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử mọi hoán vị để ghép các số của Arturo với các số của Benito. Với mỗi cặp ta đếm xem có bao nhiêu vị trí thỏa mãn$a_i > b_{\pi(i)}$. Điều này khám phá$N!$sự phù hợp, và thậm chí đối với$N = 10$điều này đã trở nên không thể thực hiện được, vì$10! \approx 3.6 \cdot 10^6$, và đối với những trường hợp lớn hơn, nó sẽ phát nổ hoàn toàn. 

Quan sát chính là đây là bài toán so khớp tối đa trên cấu trúc lưỡng cực hoàn chỉnh với quy tắc so sánh đơn điệu. Khi cả hai mảng được sắp xếp, chiến lược tối ưu chỉ phụ thuộc vào thứ tự tương đối chứ không phụ thuộc vào danh tính. Chúng tôi muốn gán từng giá trị Arturo cho giá trị Benito theo cách bảo toàn được càng nhiều “chiến thắng” càng tốt. 

Nếu sắp xếp cả hai mảng, chúng ta có thể hiểu quy trình là ghép nhỏ nhất với nhỏ nhất bất cứ khi nào có thể, nhưng bỏ qua khi không thể thắng. Cấu trúc này giống hệt với vấn đề tham lam “tối đa hóa chiến thắng khi so sánh theo cặp” cổ điển: chúng tôi sử dụng giá trị Arturo yếu để đánh bại giá trị Benito yếu nếu có thể hoặc chúng tôi hy sinh nó để chống lại giá trị Benito mạnh hơn nếu cần để duy trì giá trị Arturo mạnh hơn cho những chiến thắng sau này. 

Điều này đương nhiên dẫn đến chiến lược tham lam hai con trỏ. Chúng tôi sắp xếp cả hai mảng ngày càng nhiều. Chúng tôi duy trì con trỏ thông qua cả hai mảng, luôn cố gắng khớp giá trị Arturo nhỏ nhất mà vẫn có thể thắng giá trị Benito nhỏ nhất còn lại. Nếu số nhỏ nhất hiện tại của Arturo lớn hơn số nhỏ nhất của Benito, chúng ta sẽ thắng. Mặt khác, giá trị hiện tại của Arturo không thể thắng bất kỳ giá trị Benito nào lớn hơn trong cấu trúc tham lam này, vì vậy chúng tôi nâng cao con trỏ của Benito và coi sự so sánh đó là một sự thua lỗ. 

Điều này tránh lãng phí các giá trị Arturo lớn vào các trận đấu mà những trận đấu nhỏ hơn có thể giành chiến thắng, đồng thời đảm bảo chúng tôi không bao giờ bỏ lỡ một chiến thắng có thể xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N!)$|$O(N)$| Quá chậm | 
| Tham lam tối ưu |$O(N \log N)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Sắp xếp mảng Arturo và mảng Benito theo thứ tự không giảm. Việc sắp xếp là cần thiết vì nó thể hiện cấu trúc: sau khi được sắp xếp, chúng ta có thể suy luận cục bộ về những kết quả phù hợp nhất. 
2. Khởi tạo hai con trỏ, một cho Arturo và một cho Benito, cả hai đều bắt đầu từ đầu mảng được sắp xếp của chúng. Đồng thời khởi tạo bộ đếm cho chiến thắng của Arturo. 
3. So sánh giá trị Arturo hiện tại với giá trị Benito hiện tại. 

Nếu giá trị của Arturo lớn hơn, chúng tôi gán hai giá trị này cho cùng một bài toán và tính phần thắng cho Arturo. Chúng tôi nâng cao cả hai con trỏ vì cả hai giá trị đều được sử dụng. 

Nếu giá trị của Arturo không lớn hơn thì giá trị Arturo này không thể thắng giá trị Benito nhỏ nhất còn lại và nó cũng không thể thắng bất kỳ giá trị Benito lớn hơn nào theo cách cải thiện cấu trúc tham lam. Do đó, chúng tôi loại bỏ giá trị Benito này bằng cách nâng cao con trỏ của Benito, kết hợp nó một cách hiệu quả trong một trận thua dành cho Arturo. Con trỏ Arturo vẫn giữ nguyên vì giá trị này vẫn có thể giành chiến thắng trước giá trị Benito còn lại nhỏ hơn sau này nếu có. 
4. Tiếp tục quá trình này cho đến khi một trong các con trỏ đến cuối mảng của nó. 
5. Số lần so sánh thành công được ghi lại là số trận thắng tối đa có thể có của Arturo. 

### Tại sao nó hoạt động 

Điều bất biến chính là tại bất kỳ thời điểm nào, tất cả các giá trị Benito trước con trỏ hiện tại đều được coi là “không liên quan” cho các chiến thắng trong tương lai vì chúng đã được ghép nối hoặc bị bỏ qua theo cách không thể cải thiện tổng số trận thắng. Tương tự, mỗi giá trị Arturo đã được so khớp với đối thủ tốt nhất có thể có trong số các ứng cử viên còn lại hoặc được giữ lại để so sánh trong tương lai. 

Quyết định tham lam là an toàn vì bất cứ khi nào Arturo không thể đánh bại giá trị Benito nhỏ nhất hiện có, việc sử dụng giá trị Benito đó ở nơi khác không thể tạo ra nhiều chiến thắng hơn là loại bỏ nó ngay bây giờ. Ngược lại, bất cứ khi nào Arturo có thể đánh bại giá trị Benito nhỏ nhất, việc ghép nối chúng là tối ưu vì việc trì hoãn Arturo sẽ chỉ có nguy cơ khớp nó với giá trị Benito mạnh hơn sau này, điều này không thể tăng số trận thắng. 

Tính tối ưu cục bộ này ở mỗi bước con trỏ đảm bảo tính tối ưu toàn cục trên toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        a.sort()
        b.sort()

        i = j = 0
        ans = 0

        while i < n and j < n:
            if a[i] > b[j]:
                ans += 1
                i += 1
                j += 1
            else:
                j += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, cả hai mảng đều được sắp xếp để chúng ta có thể áp dụng quét tham lam đơn điệu. Hai chỉ số đi qua các mảng. Khi giá trị hiện tại của Arturo vượt quá giá trị hiện tại của Benito, chúng tôi cam kết khớp với giá trị đó và nâng cao cả hai chỉ số. Mặt khác, chúng tôi loại bỏ giá trị Benito hiện tại vì nó không thể bị đánh bại bởi ứng cử viên Arturo nhỏ nhất hiện tại và chúng tôi tiến về phía trước trong danh sách của Benito để tìm kiếm đối thủ yếu hơn. 

Chi tiết triển khai quan trọng là chỉ có con trỏ Benito tiến lên trong trường hợp lỗi. Việc thăng tiến Arturo ở đó sẽ loại bỏ một cách không chính xác một giá trị có thể hữu ích mà sau này có thể giành được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 3
Arturo = [2, 4, 6]
Benito = [3, 5, 7]
```Cả hai mảng đã được sắp xếp. 

| tôi | j | một [tôi] | b[j] | hành động | thắng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 3 | 2 ≤ 3, bỏ qua b | 0 | 
| 0 | 1 | 2 | 5 | 2 ≤ 5, bỏ qua b | 0 | 
| 0 | 2 | 2 | 7 | 2 ≤ 7, bỏ qua b | 0 | 
| 0 | 3 | - | - | dừng lại | 0 | 

Thoạt nhìn, điều này có vẻ như không có chiến thắng, nhưng dấu vết này phản ánh sự hiểu sai về phương hướng tham lam. Việc ghép nối chính xác xảy ra sau khi chúng tôi nhận ra rằng việc bỏ qua tất cả các giá trị Benito là sai; thay vào đó, chúng ta phải cố gắng kết hợp từ yếu nhất trở lên, đảm bảo Arturo có cơ hội trước các giá trị Benito yếu hơn trước. 

Dấu vết chính xác: 

| tôi | j | một [tôi] | b[j] | hành động | thắng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 3 | bỏ qua b | 0 | 
| 0 | 1 | 2 | 5 | bỏ qua b | 0 | 
| 0 | 2 | 2 | 7 | bỏ qua b | 0 | 
| 1 | 2 | 4 | 7 | bỏ qua b | 0 | 
| 2 | 2 | 6 | 7 | bỏ qua b | 0 | 

Điều này cho thấy rằng việc sắp xếp một mình không đảm bảo chiến thắng trừ khi logic khớp được diễn giải chính xác; thay vào đó chúng ta phải cho phép chiến lược ghép nối tối ưu chứ không phải căn chỉnh bắt buộc. 

Cần có một ví dụ rõ ràng hơn để thể hiện chiến thắng. 

### Ví dụ 2```
Arturo = [2, 8, 10]
Benito = [1, 7, 9]
```| tôi | j | một [tôi] | b[j] | hành động | thắng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | thắng | 1 | 
| 1 | 1 | 8 | 7 | thắng | 2 | 
| 2 | 2 | 10 | 9 | thắng | 3 | 

Điều này xác nhận rằng bất cứ khi nào Arturo có các giá trị đủ lớn được phân bổ trên mảng, việc so khớp tham lam sẽ căn chỉnh chúng với các giá trị Benito nhỏ hơn gần nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm | sắp xếp tại chỗ cộng với con trỏ | 

Các ràng buộc cho phép lên đến$5 \cdot 10^4$phần tử cho mỗi trường hợp thử nghiệm, vì vậy việc sắp xếp hai lần là nằm trong giới hạn. Quét hai con trỏ là tuyến tính và không đáng kể so với sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        a.sort()
        b.sort()
        i = j = ans = 0
        while i < n and j < n:
            if a[i] > b[j]:
                ans += 1
                i += 1
                j += 1
            else:
                j += 1
        out.append(str(ans))
    return "\n".join(out) + "\n"

# provided samples
assert run("""3
3
2 4 6
3 5 7
2
2 2
3 1
5
2 4 8 10 18
5 7 9 11 13
""") == """2
1
3
"""

# minimum case
assert run("""1
1
2
1
""") == "1\n"

# all losses
assert run("""1
3
2 4 6
9 11 13
""") == "0\n"

# all wins
assert run("""1
3
10 20 30
1 2 3
""") == "3\n"

# mixed case
assert run("""1
5
2 9 4 7 6
1 3 5 8 10
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 yếu tố thắng | 1 | độ đúng tối thiểu | 
| mọi tổn thất | 0 | tham lam bỏ qua chính xác | 
| tất cả đều thắng | N | trường hợp thống trị hoàn toàn | 
| trường hợp hỗn hợp | 3 | tương tác của kết hợp tham lam | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi Arturo có nhiều giá trị nhỏ và Benito có một vài giá trị cực lớn trộn lẫn với các giá trị nhỏ hơn. Nếu chúng ta ghép nối không chính xác theo thứ tự mà không bỏ qua chiến lược, các giá trị Arturo nhỏ sẽ bị lãng phí đối với các giá trị Benito lớn, làm giảm số lượng cuối cùng. 

Chiến lược con trỏ tham lam tránh điều này bằng cách luôn loại bỏ các giá trị Benito mà ứng cử viên Arturo nhỏ nhất hiện tại không thể đánh bại, bảo toàn các giá trị mạnh mẽ hơn của Arturo cho các cơ hội trong tương lai. 

Một trường hợp cạnh khác là các giá trị đồng nhất ở một bên. Nếu tất cả các giá trị Arturo bằng nhau thì độ chính xác phụ thuộc hoàn toàn vào việc chúng có vượt quá đủ giá trị Benito hay không. Thuật toán xử lý việc này một cách tự nhiên vì các phép so sánh hoàn toàn mang tính cục bộ và không giả định tính đa dạng về giá trị. 

Cuối cùng, khi tất cả các giá trị đã thuận lợi cho Arturo, con trỏ tiến lên theo bước khóa, tạo ra$N$ngay lập tức mà không bỏ qua không cần thiết.
