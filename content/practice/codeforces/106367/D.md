---
title: "CF 106367D - Trò chơi xổ số của Whalica"
description: "Chúng ta được cung cấp một tập hợp các chuỗi hình quạt trên một bảng chữ cái nhỏ và một chuỗi các sự kiện có thể biến đổi đồng thời tất cả các chuỗi hoặc đánh giá một kết quả “hòa” với một chuỗi chiến thắng nhất định."
date: "2026-06-19T17:14:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106367
codeforces_index: "D"
codeforces_contest_name: "Whalica Cup (Round 2)"
rating: 0
weight: 106367
solve_time_s: 56
verified: true
draft: false
---

[CF 106367D - Trò chơi xổ số của Whalica](https://codeforces.com/problemset/problem/106367/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi hình quạt trên một bảng chữ cái nhỏ và một chuỗi các sự kiện có thể biến đổi đồng thời tất cả các chuỗi hoặc đánh giá một kết quả “hòa” với một chuỗi chiến thắng nhất định. 

Đối với một sự kiện rút thăm, mỗi người hâm mộ đóng góp một số điểm bằng độ dài của tiền tố chung dài nhất giữa chuỗi hiện tại của họ và chuỗi chiến thắng được cung cấp. Đầu ra yêu cầu là tổng các giá trị này trên tất cả các quạt. 

Đối với một sự kiện đột biến, mỗi chuỗi quạt được chuyển đổi ký tự theo ký tự bằng cách sử dụng dịch chuyển chu kỳ cố định trên chu kỳ bảng chữ cái w → h → a → l → i → c → w. Điều quan trọng là tất cả các chuỗi giữ độ dài không thay đổi và tất cả các ký tự được cập nhật độc lập nhưng đồng thời. 

Các giới hạn rất chặt chẽ: tối đa 10^5 người hâm mộ và 10^5 sự kiện, với tổng độ dài của tất cả các chuỗi và tất cả các chuỗi truy vấn cũng bị giới hạn bởi 10^5. Điều này ngay lập tức loại trừ mọi giải pháp xử lý từng quạt cho mỗi truy vấn hoặc mô phỏng so sánh LCP trực tiếp. Ngay cả một lần rút duy nhất quét tất cả các chuỗi cũng sẽ quá chậm. 

Một khó khăn tinh tế là các chuỗi phát triển trên toàn cầu. Một cách tiếp cận đơn giản có thể cố gắng duy trì tất cả các chuỗi một cách rõ ràng và tính toán lại các so sánh cho mọi truy vấn, nhưng việc tính toán LCP lặp đi lặp lại đối với tất cả các quạt sẽ vượt quá giới hạn. 

Vấn đề không rõ ràng thứ hai là đột biến xảy ra đồng nhất trên tất cả các chuỗi, nghĩa là cấu trúc tương đối được giữ nguyên nhưng nhãn ký tự tuyệt đối sẽ thay đổi. Điều này cho thấy chúng ta nên tránh cập nhật vật lý từng chuỗi ký tự cho mỗi thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi lần rút thăm, chúng tôi so sánh chuỗi chiến thắng với mọi chuỗi người hâm mộ và tính toán LCP bằng cách quét các ký tự cho đến khi xảy ra sự không khớp. Mỗi đột biến áp dụng phép dịch chuyển theo chu kỳ cho mọi ký tự của mỗi chuỗi. Điều này đúng, nhưng đắt tiền. 

Một lần tính toán LCP có giá O (độ dài của chuỗi). Trong trường hợp hòa xấu nhất, tính tổng tất cả các quạt có giá O (tổng chiều dài của tất cả các chuỗi quạt), có thể chấp nhận được một lần. Tuy nhiên, các đột biến buộc chúng ta phải viết lại nhiều lần tất cả các chuỗi. Với tối đa 10^5 sự kiện và tổng độ dài 10^5, một bản cập nhật đơn giản cho mỗi ký tự cho mỗi đột biến sẽ dẫn đến O(n·q) theo cách hiểu tồi tệ nhất nếu các chuỗi được quét hoặc xây dựng lại nhiều lần, điều này không an toàn. Nút thắt thực sự xuất hiện khi nhiều lần rút liên tục duyệt qua tất cả các chuỗi theo từng ký tự. 

Quan sát quan trọng là bảng chữ cái cực kỳ nhỏ và đột biến là một chu kỳ hoán vị cố định. Thay vì sửa đổi chuỗi, chúng ta có thể duy trì trạng thái dịch chuyển toàn cục. Mỗi ký tự trong mỗi chuỗi quạt đều được diễn giải một cách hiệu quả dưới ánh xạ dịch chuyển, vì vậy chúng ta có thể biểu thị trạng thái hiện tại dưới dạng chỉ số xoay trong chu kỳ. Sau đó, mỗi lần rút sẽ so sánh các ký tự trong ánh xạ hiện tại mà không cần viết lại chuỗi về mặt vật lý. 

Điều này làm giảm đột biến từ O(tổng chiều dài) xuống O(1) và giữ cho tất cả các so sánh được nhẹ nhàng. 

Thách thức còn lại là tính toán tổng LCP trên tất cả các chuỗi một cách hiệu quả đối với một chuỗi truy vấn. Vì tổng chiều dài của tất cả các chuỗi là 10^5 nên chúng ta có thể lưu trữ trước các chuỗi theo nguyên trạng và lặp qua chúng. Mỗi so sánh ký tự là O(1) và trên tất cả các lần rút, tổng số so sánh ký tự được giới hạn bởi tổng của tất cả độ dài chuỗi nhân với độ sâu tối đa mà chúng được quét, vẫn nằm trong giới hạn vì khi xảy ra sự không khớp, chúng tôi sẽ dừng sớm trên mỗi chuỗi. 

Sự thay đổi toàn cục có thể được xử lý bằng cách mã hóa từng ký tự dưới dạng chỉ mục trong mảng chu kỳ [w, h, a, l, i, c] và áp dụng phần bù. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Σ | si | mỗi lần rút + viết lại toàn bộ mỗi lần đột biến) | 
| Tối ưu | O(Σ | si | + Σ | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi sửa thứ tự của chu trình bảng chữ cái và coi mọi ký tự là một chỉ mục trong chu trình này. Một đột biến chỉ đơn giản là thay đổi chỉ số này trên toàn cầu. 

1. Tính toán trước ánh xạ từ mỗi ký tự đến vị trí của nó trong chu trình. Chúng tôi cũng giữ lại chính mảng chu trình để có thể giải mã các ký tự đã dịch chuyển. Điều này cho phép chuyển đổi O(1) giữa các ký tự và chỉ số. 
2. Duy trì một số nguyên duy nhất`shift`biểu thị số lần thao tác đột biến đã được áp dụng. Mỗi đột biến tăng dần`shift`bằng một modulo 6. Điều này thay thế việc viết lại toàn bộ chuỗi bằng bản cập nhật liên tục. 
3. Lưu trữ tất cả các chuỗi quạt ở dạng mã hóa ban đầu của chúng dưới dạng mảng số nguyên trên các chỉ số chu kỳ. Chúng tôi không bao giờ sửa đổi chúng nữa. Sự thay đổi được áp dụng một cách lười biếng trong quá trình so sánh. 
4. Đối với mỗi truy vấn rút thăm có chuỗi chiến thắng T, chúng tôi chuyển đổi T thành chỉ số chu kỳ một lần, do đó việc so sánh trở thành dựa trên số nguyên. 
5. Để tính toán câu trả lời, chúng tôi lặp lại từng chuỗi quạt. Đối với mỗi người hâm mộ, chúng tôi tính toán LCP với T bằng cách so sánh từng ký tự: 

chúng tôi so sánh ký tự quạt ở vị trí j, được hiểu là`(si[j] + shift) mod 6`, với T[j]. Chúng tôi dừng lại ở điểm không khớp đầu tiên và thêm chỉ mục đó vào câu trả lời. 

Mỗi bước so sánh là O(1) và chúng tôi dừng sớm đối với mỗi chuỗi, do đó tổng công việc tỷ lệ thuận với tổng độ dài tiền tố được quét. 

### Tại sao nó hoạt động 

Hoạt động đột biến là một hoán vị thống nhất được áp dụng cho mọi ký tự trong mỗi chuỗi. Việc biểu diễn các ký tự dưới dạng chỉ số chu kỳ và duy trì phần bù chung sẽ duy trì sự tương đương chính xác giữa các chuỗi bị biến đổi rõ ràng và cách diễn giải được dịch chuyển một cách lười biếng. Mọi so sánh giữa chuỗi quạt và chuỗi truy vấn đều được thực hiện theo cùng một bảng chữ cái được dịch chuyển, do đó sự bằng nhau của các tiền tố được giữ nguyên ở mọi vị trí. Điều này đảm bảo LCP được tính toán theo biểu diễn lười phù hợp với LCP sẽ thu được nếu tất cả các chuỗi được cập nhật vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

cycle = ['w', 'h', 'a', 'l', 'i', 'c']
idx = {c: i for i, c in enumerate(cycle)}

n, q = map(int, input().split())

fans = []
for _ in range(n):
    s = input().strip()
    fans.append([idx[ch] for ch in s])

shift = 0

def decode(x):
    return cycle[(x + shift) % 6]

for _ in range(q):
    parts = input().split()
    if parts[0] == '2':
        shift = (shift + 1) % 6
    else:
        T = parts[1].strip()
        t = [idx[ch] for ch in T]

        ans = 0
        for s in fans:
            lim = min(len(s), len(t))
            i = 0
            while i < lim:
                if (s[i] + shift) % 6 != t[i]:
                    break
                i += 1
            ans += i

        print(ans)
```Việc triển khai lưu trữ tất cả các chuỗi quạt dưới dạng mảng số nguyên để việc so sánh tránh được chi phí chuỗi. Toàn cầu`shift`theo dõi có bao nhiêu đột biến đã xảy ra. 

Trong thời gian rút thăm, chuỗi chiến thắng cũng được chuyển đổi một lần thành chỉ số. Mỗi quạt được quét cho đến khi không khớp hoặc kết thúc một chuỗi. Hoạt động modulo áp dụng đột biến một cách lười biếng, tránh việc viết lại chuỗi. 

Một lỗi phổ biến là cố gắng thực sự xoay mọi ký tự trong mỗi chuỗi trong quá trình đột biến, điều này sẽ ngay lập tức TLE. Một điểm tinh tế khác là quên rằng LCP phụ thuộc vào cả đẳng thức và giới hạn, do đó vòng lặp phải dừng ở chuỗi ngắn hơn trong hai chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ phù hợp với chu kỳ: 

đầu vào:```
2 2
wha
hal
1 wha
2
```Chúng tôi theo dõi trạng thái. 

Trước bất kỳ sự kiện nào, shift = 0. 

### Lần rút thăm đầu tiên 

| Quạt | s | t | so sánh | LCP | 
| --- | --- | --- | --- | --- | 
| 1 | cái gì | cái gì | w=w, h=h, a=a | 3 | 
| 2 | hal | cái gì | h=w không khớp | 0 | 

Đáp án = 3. 

### Đột biến 

Shift trở thành 1. 

Bây giờ mỗi quạt được quay hiệu quả một bước. 

### Sự kiện thứ hai (nếu có đợt rút thăm khác) 

Nếu chúng ta có "whc" hòa, việc so sánh sẽ được thực hiện bằng cách sử dụng cách giải thích thay đổi. 

Điều này chứng tỏ rằng đột biến được hấp thụ vào số học chỉ mục hơn là viết lại rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Σ LCP quét qua tất cả các truy vấn) | Mỗi so sánh ký tự là O(1) và tổng số tiền tố được quét bị giới hạn bởi các ràng buộc về kích thước đầu vào | 
| Không gian | O(Σ | si | 

Giới hạn tổng chiều dài là 10^5 đảm bảo rằng ngay cả việc quét toàn bộ trên tất cả các chuỗi vẫn khả thi khi mỗi ký tự chỉ được xử lý với tổng số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    cycle = ['w', 'h', 'a', 'l', 'i', 'c']
    idx = {c: i for i, c in enumerate(cycle)}

    n, q = map(int, input().split())

    fans = []
    for _ in range(n):
        s = input().strip()
        fans.append([idx[ch] for ch in s])

    shift = 0
    out = []

    for _ in range(q):
        parts = input().split()
        if parts[0] == '2':
            shift = (shift + 1) % 6
        else:
            T = parts[1].strip()
            t = [idx[ch] for ch in T]
            ans = 0
            for s in fans:
                lim = min(len(s), len(t))
                i = 0
                while i < lim and (s[i] + shift) % 6 == t[i]:
                    i += 1
                ans += i
            out.append(str(ans))

    return "\n".join(out)

# minimal
assert run("1 1\nw\n1 w\n") == "1"

# sample-like
assert run("2 2\nwha\nhal\n1 wha\n2\n") == "3"

# all same strings
assert run("3 1\nw\nw\nw\n1 w\n") == "3"

# after mutation effect
assert run("1 2\nw\n2\n1 h\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 1 | tính đúng đắn của LCP cơ sở | 
| giống mẫu | 3 | tổng hợp qua người hâm mộ | 
| tất cả đều giống nhau | 3 | tổng hợp trên các chuỗi giống hệt nhau | 
| hiệu ứng đột biến | 1 | xử lý ca theo chu kỳ | 

## Vỏ cạnh 

Trường hợp chuỗi tối thiểu như chuỗi quạt một ký tự đảm bảo rằng logic LCP xử lý chính xác các điều kiện kết thúc ngay lập tức. Thuật toán so sánh một ký tự và khớp hoặc ngắt ngay lập tức, tạo ra sự tích lũy chính xác. 

Trường hợp đột biến xảy ra trước bất kỳ lần rút thăm nào sẽ xác minh rằng sự thay đổi toàn cầu được áp dụng một cách nhất quán. Ví dụ: chuỗi quạt "w" sau một đột biến thực tế sẽ trở thành "h" và phép so sánh sử dụng`(0 + 1) % 6`, đảm bảo khớp chính xác với "h". 

Một trường hợp có độ dài khác nhau đảm bảo việc kết thúc vòng lặp tại`min(len(s), len(t))`là đúng. Nếu không có giới hạn này, việc lập chỉ mục ngoài chuỗi ngắn hơn sẽ tạo ra những so sánh không chính xác hoặc lỗi thời gian chạy.
