---
title: "CF 1538E - Chuỗi con vui nhộn"
description: "Chúng ta được cung cấp một “ngôn ngữ lập trình” rất nhỏ nơi các biến lưu trữ các chuỗi. Mỗi biến được gán một chuỗi ký tự có độ dài tối đa là năm hoặc được định nghĩa là sự kết hợp của hai biến được xác định trước đó."
date: "2026-06-16T15:08:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "implementation", "matrices", "strings"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 2100
weight: 1538
solve_time_s: 399
verified: true
draft: false
---

[CF 1538E - Chuỗi con vui nhộn](https://codeforces.com/problemset/problem/1538/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, băm, triển khai, ma trận, chuỗi 
**Thời gian giải:** 6 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một “ngôn ngữ lập trình” rất nhỏ nơi các biến lưu trữ các chuỗi. Mỗi biến được gán một chuỗi ký tự có độ dài tối đa là năm hoặc được định nghĩa là sự kết hợp của hai biến được xác định trước đó. Chương trình là một chuỗi các nhiệm vụ như vậy và chỉ có giá trị của biến cuối cùng mới quan trọng. 

Nhiệm vụ của chúng ta không phải là tái tạo lại chuỗi cuối cùng một cách rõ ràng mà là tính xem chuỗi con đó bao nhiêu lần`"haha"`xuất hiện trong đó, bao gồm cả những lần xuất hiện chồng chéo. 

Hạn chế quan trọng là các chuỗi có thể tăng chiều dài theo cấp số nhân do nối nhiều lần. Mặc dù mỗi chuỗi cơ sở rất nhỏ nhưng được lặp đi lặp lại`x = a + b`các hoạt động có thể nhanh chóng tạo ra các chuỗi quá lớn để hiện thực hóa. Do đó, việc mô phỏng trực tiếp là không thể. 

Khó khăn tinh tế đến từ các hiệu ứng chồng chéo. Khi nối hai chuỗi`A + B`, lần xuất hiện của`"haha"`có thể xuất hiện hoàn toàn ở`A`, hoàn toàn trong`B`, hoặc nằm giữa ranh giới giữa chúng. Hành vi ranh giới đó chỉ phụ thuộc vào hậu tố của`A`và tiền tố của`B`và không có nội dung đầy đủ của chúng. 

Các trường hợp khó phá vỡ các cách tiếp cận ngây thơ đều liên quan đến ranh giới này: 

Chế độ thất bại đầu tiên là bỏ qua sự chồng chéo trên các ranh giới nối. Ví dụ,`"ha" + "ha"`không chứa`"haha"`bên trong ở một nửa, nhưng phép nối tạo thành một lần xuất hiện. 

Chế độ lỗi thứ hai là cố gắng chỉ duy trì số lượng mà không có đủ thông tin về tiền tố và hậu tố. Ví dụ: nếu chúng ta chỉ theo dõi có bao nhiêu`"haha"`xuất hiện bên trong mỗi biến, chúng ta mất khả năng phát hiện các lần xuất hiện mới được hình thành sau khi nối. 

Kiểu lỗi thứ ba là cắt chuỗi không chính xác. Chúng tôi không cần chuỗi đầy đủ, nhưng chúng tôi cần đủ ngữ cảnh để có thể phát hiện được mọi kết quả trùng khớp xuyên ranh giới có thể có. Vì độ dài mẫu là bốn nên việc giữ một tiền tố và hậu tố cố định nhỏ là đủ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xây dựng rõ ràng mọi chuỗi và sau đó quét nó để tìm sự xuất hiện của`"haha"`. Mỗi phép nối sẽ sao chép toàn bộ chuỗi và việc đếm sẽ yêu cầu quét tuyến tính. Vì kích thước chuỗi có thể tăng gấp đôi ở mỗi bước, sau`n`độ dài chuỗi có thể đạt tới kích thước theo cấp số nhân, làm cho phương pháp này không khả thi ngay cả ở mức tối đa`n = 50`. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến các kết quả khớp có mẫu cố định có độ dài bằng 4. Bất kỳ sự xuất hiện của`"haha"`nằm hoàn toàn bên trong một cửa sổ cục bộ gồm bốn ký tự. Khi nối hai chuỗi, bất kỳ sự xuất hiện mới nào vượt qua ranh giới phải bắt đầu bằng ba ký tự cuối cùng của chuỗi bên trái và mở rộng thành ba ký tự đầu tiên của chuỗi bên phải. Vì vậy, chỉ cần duy trì ba thông tin cho mỗi biến là đủ: số lượng`"haha"`xuất hiện bên trong nó, tiền tố có tối đa ba ký tự và hậu tố có tối đa ba ký tự. 

Với cách biểu diễn này, phép nối trở thành thời gian không đổi: số lượng nội bộ được thêm vào và các điểm giao cắt ranh giới được tính toán bằng cách kiểm tra tối đa ba vị trí chồng chéo giữa hậu tố và tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng chuỗi Brute Force | O(tổng kích thước chuỗi) | O(tổng kích thước chuỗi) | Quá chậm | 
| DP với số lượng + cắt tiền tố/hậu tố | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các câu lệnh theo thứ tự, duy trì cho mỗi biến một trạng thái nhỏ gọn bao gồm ba thành phần: số lượng`"haha"`lần xuất hiện, tối đa ba ký tự đầu tiên của chuỗi và tối đa ba ký tự cuối cùng. 

1. Nếu một câu lệnh có dạng`x := s`, chúng ta trực tiếp khởi tạo biến. Chúng tôi đếm số lần xuất hiện của`"haha"`TRONG`s`bằng vũ lực kể từ đó`|s| ≤ 5`và lưu trữ tiền tố và hậu tố (cắt ngắn theo độ dài 3). Điều này an toàn vì sợi dây nhỏ. 
2. Nếu một tuyên bố là`x = a + b`, chúng tôi kết hợp các trạng thái được lưu trữ của`a`Và`b`. Tổng số nội bộ bắt đầu như`cnt[a] + cnt[b]`. 
3. Sau đó chúng tôi tính toán các lần xuất hiện xuyên biên giới. Từ`"haha"`có độ dài 4, bất kỳ sự giao cắt nào cũng phải được hình thành bằng cách lấy hậu tố của`a`và tiền tố của`b`. Chúng tôi thử tất cả các phần chia nơi chúng tôi thực hiện`k`các ký tự từ cuối`a`(từ 1 đến 3) và`4-k`các ký tự từ đầu`b`, kiểm tra xem phép nối có bằng không`"haha"`. Mỗi trận đấu sẽ tăng số lượng. 
4. Chúng ta xây dựng tiền tố mới bằng cách lấy`a.prefix + b.prefix`và cắt tỉa theo chiều dài tối đa 3. 
5. Chúng ta xây dựng hậu tố mới tương tự từ`a.suffix + b.suffix`, cũng cắt theo chiều dài tối đa là 3. 
6. Câu trả lời cuối cùng là số lượng được lưu trong biến được sử dụng trong câu lệnh cuối cùng. 

Lý do điều này có tác dụng là vì mỗi lần xuất hiện của`"haha"`được chứa hoàn toàn bên trong một trong các thành phần hoặc nó vượt qua ranh giới nối. Bất kỳ sự xuất hiện kéo dài nào cũng phải giao nhau với ranh giới tối đa 3 ký tự từ mỗi bên, vì độ dài mẫu là 4. Bộ lưu trữ tiền tố và hậu tố của chúng tôi đảm bảo rằng chúng tôi không bao giờ mất bất kỳ thông tin nào cần thiết để phát hiện những sự trùng lặp như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def calc_small(s):
    cnt = 0
    for i in range(len(s) - 3):
        if s[i:i+4] == "haha":
            cnt += 1
    return cnt

def merge(a, b):
    cnt = a[0] + b[0]

    A = a[1]
    B = b[2]

    # cross boundary check
    for k in range(1, 4):
        if k <= len(a[2]) and (4 - k) <= len(b[1]):
            if a[2][-k:] + b[1][:4 - k] == "haha":
                cnt += 1

    new_pref = (a[1] + b[1])[:3]
    new_suf = (a[2] + b[2])[-3:]

    return (cnt, new_pref, new_suf)

t = int(input())
for _ in range(t):
    n = int(input())
    mp = {}

    last_var = None

    for _ in range(n):
        parts = input().strip().split()

        if ":=" in parts:
            x = parts[0]
            s = parts[2]
            cnt = calc_small(s)
            mp[x] = (cnt, s[:3], s[-3:])
            last_var = x
        else:
            x = parts[0]
            a = parts[2]
            b = parts[4]
            mp[x] = merge(mp[a], mp[b])
            last_var = x

    print(mp[last_var][0])
```Việc triển khai dựa vào việc nén mọi chuỗi thành chữ ký có kích thước không đổi. các`calc_small`là an toàn vì chuỗi cơ sở có độ dài tối đa là năm. các`merge`là cốt lõi: nó chỉ kiểm tra tối đa ba ký tự từ mỗi bên, đủ để phát hiện tất cả các lần xuất hiện ranh giới của mẫu bốn ký tự. 

Một cạm bẫy phổ biến là việc lặp lại không chính xác qua các phần phân chia ranh giới. Chúng tôi đã kiểm tra rõ ràng tất cả các cách mà mẫu 4 ký tự có thể được chia thành hai chuỗi, đảm bảo không bỏ sót trường hợp trùng lặp nào. Một điểm tinh tế khác là giữ tiền tố và hậu tố giới hạn ở độ dài ba; việc lưu trữ lâu hơn là không cần thiết và sẽ có nguy cơ xảy ra hiện tượng O(n²) ngẫu nhiên. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên trong đó các biến phát triển và nối lại nhiều lần. 

Để đơn giản, chỉ theo dõi biến có ý nghĩa cuối cùng`d`. 

| Bước | Hoạt động | Đếm | Tiền tố | Hậu tố | 
| --- | --- | --- | --- | --- | 
| một |`"h"`| 0 | h | h | 
| b |`"aha"`| 0 | à ha | à ha | 
| c | a + b | 1 | h+aha →`hah`| h+aha →`aha`| 
| c | c + c | 3 | ha + ha →`hah`| aha + aha →`aha`| 
| e | c + c | 3 | ha | à ha | 
| d | a + c | 3 | h + ha →`hah`| h + aha →`aha`| 

Dấu vết này cho thấy hầu hết sự tăng trưởng đều không liên quan ngoại trừ các ranh giới gần. Số lượng chỉ tăng khi hình thành các kết hợp xuyên biên giới`"haha"`. 

Bây giờ hãy xem xét trường hợp tăng trưởng theo cấp số nhân trong đó`x := haha`được nhân đôi nhiều lần. Mỗi cấu trúc bảo tồn nhân đôi và sự chồng chéo xuyên biên giới đóng góp một số lượng lớn các lần xuất hiện mới, tạo ra sự tăng trưởng theo cấp số nhân trong khi kích thước biểu diễn không đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi câu lệnh thực hiện một công việc không đổi, vì việc hợp nhất chỉ kiểm tra một số trường hợp biên cố định | 
| Không gian | O(n) | Chúng tôi lưu trữ một trạng thái có kích thước không đổi cho mỗi biến | 

Các ràng buộc cho phép tối đa 50 câu lệnh cho mỗi trường hợp thử nghiệm và 1000 trường hợp thử nghiệm, vì vậy giải pháp này dễ dàng nằm trong giới hạn. Mỗi thao tác là thời gian không đổi và không có chuỗi nào vượt quá độ dài 3 ở dạng được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def calc_small(s):
        cnt = 0
        for i in range(len(s) - 3):
            if s[i:i+4] == "haha":
                cnt += 1
        return cnt

    def merge(a, b):
        cnt = a[0] + b[0]
        for k in range(1, 4):
            if k <= len(a[2]) and (4 - k) <= len(b[1]):
                if a[2][-k:] + b[1][:4-k] == "haha":
                    cnt += 1
        return (cnt, (a[1] + b[1])[:3], (a[2] + b[2])[-3:])

    t = int(input())
    for _ in range(t):
        n = int(input())
        mp = {}
        last = None

        for _ in range(n):
            parts = input().split()
            if ":=" in parts:
                x = parts[0]
                s = parts[2]
                mp[x] = (calc_small(s), s[:3], s[-3:])
                last = x
            else:
                x = parts[0]
                a = parts[2]
                b = parts[4]
                mp[x] = merge(mp[a], mp[b])
                last = x

        print(mp[last][0])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""4
6
a := h
b := aha
c = a + b
c = c + c
e = c + c
d = a + c
15
x := haha
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
x = x + x
1
haha := hah
5
haahh := aaaha
ahhhh = haahh + haahh
haahh = haahh + haahh
ahhhh = ahhhh + haahh
ahhaa = haahh + ahhhh
""") == """3
32767
0
0"""

# custom cases
assert run("""1
1
a := haha""") == "1", "single exact match"

assert run("""1
2
a := ha
b := ha
c = a + b""") == "1", "boundary match only"

assert run("""1
3
a := h
b := a + a
c = b + b""") == "0", "no possible match"

assert run("""1
1
x := aaaaa""") == "0", "no pattern present"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu chính xác duy nhất | 1 | đếm chuỗi cơ sở trực tiếp | 
| chỉ khớp ranh giới | 1 | tính chính xác của việc phát hiện xuyên biên giới | 
| không có trận đấu nào có thể | 0 | tính đúng đắn trong trường hợp không xảy ra | 
| không có mẫu | 0 | xử lý an toàn các chuỗi không liên quan | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một lần xuất hiện đầy đủ được phân chia chính xác trên ranh giới nối. Đối với đầu vào`a := ha`Và`b := ha`, cả hai bên đều không chứa`"haha"`, nhưng sự ghép nối của chúng tạo thành một lần xuất hiện hợp lệ. Thuật toán xử lý vấn đề này bằng cách kiểm tra rõ ràng tất cả các phân chia ranh giới bằng cách sử dụng kết hợp hậu tố và tiền tố. 

Một trường hợp khác là nhân đôi lặp đi lặp lại, trong đó một chuỗi ngắn tăng nhanh về kích thước khái niệm. Ví dụ, bắt đầu từ`x := haha`và áp dụng nhiều lần`x = x + x`, số lần xuất hiện tăng theo cấp số nhân. Thuật toán xử lý việc này vì nó không bao giờ mở rộng chuỗi và mỗi lần nhân đôi đều được xử lý trong thời gian không đổi bằng cách sử dụng logic chồng chéo tiền tố-hậu tố. 

Trường hợp thứ ba là các chuỗi hoàn toàn không chứa mẫu nhưng vẫn ảnh hưởng đến kết quả khớp trong tương lai thông qua cấu trúc tiền tố hậu tố. Ngay cả khi số lượng nội bộ bằng 0, tiền tố và hậu tố được lưu trữ vẫn cần thiết và thuật toán bảo tồn chúng độc lập với số lần xuất hiện, đảm bảo các phép nối trong tương lai vẫn được đánh giá chính xác.
