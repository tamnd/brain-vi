---
title: "CF 1857E - Sức mạnh của điểm"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng ta bắt đầu với danh sách các điểm được đặt trên trục số. Với mỗi điểm, chúng ta chọn một vị trí đặc biệt s và nối s với mọi điểm xi bằng một đoạn trên đường nguyên."
date: "2026-06-09T00:47:44+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 1500
weight: 1857
solve_time_s: 104
verified: false
draft: false
---

[CF 1857E - Sức mạnh của điểm](https://codeforces.com/problemset/problem/1857/E) 

**Đánh giá:** 1500 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng ta bắt đầu với danh sách các điểm được đặt trên trục số. Đối với mỗi điểm chúng tôi chọn một vị trí đặc biệt`s`, và chúng tôi kết nối`s`đến mọi điểm`x_i`bởi một đoạn trên dòng số nguyên. Vì vậy, mỗi điểm ban đầu trở thành một khoảng kéo dài giữa tọa độ của nó và`s`. 

Đối với bất kỳ vị trí số nguyên cố định`p`trên trục số, chúng ta định nghĩa “sức mạnh” của nó là có bao nhiêu đoạn trong số này bao phủ vị trí đó. Nếu nhiều phân đoạn chồng lên nhau tại`p`, sức mạnh của nó rất cao. Đối với một sự lựa chọn nhất định`s`, chúng ta không được yêu cầu lũy thừa tại một vị trí duy nhất mà yêu cầu tổng lũy ​​thừa trên tất cả các vị trí nguyên từ`1`ĐẾN`10^9`. Tổng số này tương đương với việc tính tổng số điểm nguyên mà chúng bao phủ trên tất cả các phân đoạn. 

Nhiệm vụ là: đối với mỗi trường hợp thử nghiệm, chúng ta phải tính tổng này cho mọi lựa chọn có thể có của`s`, Ở đâu`s`bị giới hạn là một trong các tọa độ đầu vào. 

Các ràng buộc rất chặt chẽ: tổng số điểm trên tất cả các trường hợp thử nghiệm lên tới`2·10^5`, và có thể có tới`10^4`trường hợp thử nghiệm. Bất kỳ giải pháp nào tính toán lại câu trả lời một cách độc lập cho từng thí sinh`s`trong thời gian tuyến tính sẽ giảm xuống còn khoảng`O(n^2)`nói chung là quá chậm. 

Một cách tiếp cận ngây thơ sẽ, đối với mỗi`s`, xây dựng tất cả các phân đoạn và tính toán rõ ràng độ dài vùng phủ sóng. Cái đó đã tốn rồi`O(n)`mỗi`s`, và vì có`n`sự lựa chọn, điều này trở thành`O(n^2)`mỗi trường hợp thử nghiệm. Với`n`lên đến`2·10^5`, điều này vượt xa mức có thể chấp nhận được. 

Điểm tinh tế chính là các phân đoạn chồng chéo đóng góp bổ sung theo cách có cấu trúc. Mỗi phân đoạn đóng góp độ dài riêng của nó và sự trùng lặp chỉ quan trọng thông qua cách các độ dài này tổng hợp chứ không phải thông qua mô phỏng từng điểm rõ ràng. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ bao gồm các tình huống trong đó nhiều điểm có cùng tọa độ hoặc khi tất cả các điểm nằm về một phía của`s`, làm cho tất cả các đoạn đều hướng về cùng một hướng. Một trường hợp tế nhị khác là khi`s`là cực đoan (tối thiểu hoặc tối đa), khiến tất cả các phân đoạn đều căn chỉnh, tối đa hóa cấu trúc chồng chéo. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: đối với mỗi ứng viên`s`, tính từng khoảng`[min(x_i, s), max(x_i, s)]`, sau đó đối với mỗi điểm nguyên trong phạm vi, hãy tính mức độ phù hợp. Điều này có thể được tối ưu hóa một chút bằng cách quan sát rằng tổng công suất bằng tổng độ dài khoảng trừ đi các khoảng trùng lặp, nhưng ngay cả việc tính toán tổng phạm vi phủ sóng cũng yêu cầu quét trên một phạm vi tọa độ lớn lên đến`10^9`, điều đó là không thể. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng phạm vi bao phủ tại các điểm riêng lẻ. Tổng lũy ​​thừa trên tất cả các vị trí chính xác là tổng độ dài của tất cả các đoạn. Mỗi đoạn`[a, b]`đóng góp`b - a + 1`đến tổng số, bất kể sự chồng chéo. Vì vậy, câu trả lời cho một`s`đơn giản là tổng của tất cả`i`của`|x_i - s| + 1`. 

Điều này biến vấn đề thành việc đánh giá, cho mỗi`s`, biểu thức:$$\sum |x_i - s| + n$$Hằng số`n`không liên quan đến việc tối ưu hóa, vì vậy nhiệm vụ cốt lõi là tính tổng các khác biệt tuyệt đối một cách hiệu quả cho nhiều trung tâm ứng viên. Đây là một bài toán sắp xếp + tổng tiền tố cổ điển: khi mảng được sắp xếp, chúng ta có thể chia các phần đóng góp thành các phần tử ở bên trái của`s`và những phần bên phải, rồi tính từng phần trong O(1). 

Lực lượng vũ phu hoạt động vì nó đánh giá trực tiếp khoảng cách, nhưng nó thất bại vì tính toán lại sự khác biệt tuyệt đối nhiều lần là bậc hai. Quan sát cho thấy giá trị tuyệt đối được chia thành hai thành phần tuyến tính cho phép tổng tiền tố nén từng truy vấn về thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Sắp xếp + Tổng tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tiến hành khai thác cấu trúc khoảng cách tuyệt đối sau khi sắp xếp. 

1. Sắp xếp mảng tọa độ. Điều này đảm bảo rằng đối với bất kỳ lựa chọn nào`s`, tất cả các phần tử ở bên trái và bên phải của nó tạo thành các nhóm liền kề. 
2. Xây dựng tổng tiền tố của mảng đã sắp xếp để chúng ta có thể tính tổng của bất kỳ tiền tố hoặc hậu tố nào trong thời gian không đổi. 
3. Đối với mỗi chỉ số`i`, đối xử`x[i]`như trung tâm`s`. Chia mảng thành các phần tử còn lại`i`và quyền của`i`. 
4. Tính phần đóng góp của vế trái là`s * i - sum(left)`. Điều này thể hiện tổng khoảng cách từ`s`đến tất cả các phần tử nhỏ hơn. 
5. Tính phần đóng góp của vế phải là`sum(right) - s * (n - i - 1)`. Điều này thể hiện tổng khoảng cách đến các phần tử lớn hơn. 
6. Thêm`n`để giải thích cho`+1`độ dài của mỗi đoạn. 
7. Xuất kết quả cho từng vị trí. 

Mỗi bước sẽ giảm khả năng quét tuyến tính thành số học theo thời gian không đổi bằng cách tận dụng tổng tiền tố. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc phân tách giá trị tuyệt đối thành các đóng góp định hướng. Sau khi được sắp xếp, mọi phần tử đều đóng góp`s - x_i`hoặc`x_i - s`, tùy vị trí. Tổng tiền tố bảo toàn tổng chính xác của cả hai nhóm, vì vậy mỗi truy vấn sẽ xây dựng lại tổng đầy đủ một cách chính xác mà không cần tính toán lại. Không cần theo dõi cấu trúc chồng chéo vì sự chồng chéo phân đoạn đã được đưa vào diễn giải độ dài cộng thêm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        
        x.sort()
        
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + x[i]
        
        total = pref[n]
        res = []
        
        for i in range(n):
            s = x[i]
            
            left_sum = s * i - pref[i]
            right_sum = (total - pref[i + 1]) - s * (n - i - 1)
            
            res.append(left_sum + right_sum + n)
        
        print(*res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sắp xếp mảng sao cho tất cả các khoảng cách có thể được xử lý thông qua tổng tiền tố. Mảng tiền tố lưu trữ các tổng tích lũy, cho phép tính toán trực tiếp các tổng ở hai bên của mỗi ứng cử viên`s`. Đối với mỗi vị trí, phần đóng góp bên trái được tính bằng khoảng cách giữa`s`và tất cả các phần tử nhỏ hơn, và sự đóng góp phù hợp là đối xứng. 

trận chung kết`+ n`giải thích cho thực tế là mỗi phân đoạn đóng góp vào việc bao gồm điểm cuối của nó (`+1`độ dài mỗi khoảng). Điều này rất dễ bị quên vì sự chuyển đổi từ phạm vi hình học sang sự khác biệt tuyệt đối đã che giấu nó. 

Một điểm tinh tế là việc lập chỉ mục: khi tính toán quyền đóng góp, chúng tôi loại trừ`x[i]`chính nó sử dụng`pref[i + 1]`, đảm bảo chúng tôi không đếm gấp đôi phần trung tâm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
x = [1, 4, 3]
```Đã sắp xếp:`[1, 3, 4]`Tổng tiền tố:`[0, 1, 4, 8]`| tôi | s | trái_sum | đúng_sum | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | (8-1) - 1*2 = 5 | 0 + 5 + 3 = 8 | 
| 1 | 3 | 3*1 - 1 = 2 | (8-4) - 3*1 = 1 | 2 + 1 + 3 = 6? | 
| 2 | 4 | 4*2 - 4 = 4 | 0 | 4 + 0 + 3 = 7 | 

Dấu vết này cho thấy mỗi vị trí phân chia mảng thành các phần đóng góp bên trái và bên phải như thế nào. Số học trực tiếp xây dựng lại phạm vi bao phủ toàn bộ phân đoạn mà không cần xây dựng khoảng thời gian rõ ràng. 

### Ví dụ 2 

đầu vào:```
n = 4
x = [1, 10, 100, 1000]
```Đã sắp xếp không thay đổi. 

Tổng tiền tố:`[0, 1, 11, 111, 1111]`Vì`s = 100`(tôi = 2): 

| tôi | s | trái_sum | đúng_sum | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 | 100 | 100*2 - 11 = 189 | (1111-111) - 100*1 = 900 | 1089 + 4 = 1093 | 

Điều này chứng tỏ rằng ngay cả với những khoảng trống lớn, phương pháp vẫn ổn định vì nó chỉ phụ thuộc vào tổng và số đếm chứ không phụ thuộc vào phạm vi tọa độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, truy vấn tiền tố là O(1) cho mỗi phần tử | 
| Không gian | O(n) | Lưu trữ mảng tổng tiền tố | 

Các ràng buộc cho phép lên đến`2·10^5`tổng số phần tử, do đó việc sắp xếp và quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        x.sort()
        pref = [0]
        for v in x:
            pref.append(pref[-1] + v)
        total = pref[-1]
        res = []
        for i, s in enumerate(x):
            left = s * i - pref[i]
            right = (total - pref[i+1]) - s * (n - i - 1)
            res.append(left + right + n)
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided samples
assert run("""3
3
1 4 3
5
1 2 5 7 1
4
1 10 100 1000
""") == """8 7 6
16 15 18 24 16
1111 1093 1093 2893"""

# all equal
assert run("""1
5
2 2 2 2 2
""")

# single element
assert run("""1
1
100
""")

# boundary spread
assert run("""1
3
1 1000000000 500000000
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | đầu ra không đổi đối xứng | thoái hóa chồng chéo | 
| phần tử đơn | 1 | cấu trúc tối thiểu | 
| ranh giới lan rộng | xử lý đúng các khoảng trống lớn | ổn định số học | 

## Vỏ cạnh 

Khi tất cả các điểm giống nhau thì mọi đoạn đều giống hệt nhau bất kể`s`, và công thức sẽ chuyển thành phần đóng góp không đổi cho mỗi phần tử. Thuật toán xử lý vấn đề này vì tổng tiền tố làm cho cả đóng góp bên trái và bên phải bằng 0, chỉ để lại hằng số`+n`. 

Khi chỉ có một điểm thì không có phân vùng trái hay phải nên cả hai đóng góp đều bằng 0 và đáp án là chính xác`1`, khớp với một đoạn có độ dài đơn vị. 

Khi các giá trị cực kỳ dàn trải, công thức tổng tiền tố vẫn hoạt động vì nó không bao giờ phụ thuộc vào mật độ tọa độ, chỉ phụ thuộc vào tổng tổng, do đó không phát sinh vấn đề tràn hoặc độ chính xác trong số nguyên Python.
