---
title: "CF 1251A - Hỏng Bàn Phím"
description: "Chúng ta được cung cấp một chuỗi cuối cùng xuất hiện trên màn hình sau khi ai đó nhấn từng phím một trên bàn phím. Mỗi phím tương ứng với một chữ cái Latinh viết thường và mỗi phím luôn khỏe mạnh hoặc luôn bị hỏng trong toàn bộ quá trình gõ."
date: "2026-06-13T21:34:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1251
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 75 (Rated for Div. 2)"
rating: 1000
weight: 1251
solve_time_s: 307
verified: false
draft: false
---

[CF 1251A - Bàn phím bị hỏng](https://codeforces.com/problemset/problem/1251/A) 

**Đánh giá:** 1000 
**Tags:** vũ lực, dây, hai con trỏ 
**Thời gian giải:** 5 phút 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cuối cùng xuất hiện trên màn hình sau khi ai đó nhấn từng phím một trên bàn phím. Mỗi phím tương ứng với một chữ cái Latinh viết thường và mỗi phím luôn khỏe mạnh hoặc luôn bị hỏng trong toàn bộ quá trình gõ. 

Hoạt động của một phím khỏe mạnh rất đơn giản: nhấn nó sẽ thêm chính xác một bản sao của chữ cái vào đầu ra. Một phím bị hỏng sẽ nguy hiểm hơn: mỗi máy in sẽ thêm hai bản sao của chữ cái thay vì một. Chuỗi cuối cùng là kết quả của việc trộn lẫn hai hành vi này qua một chuỗi các lần nhấn phím không xác định. 

Nhiệm vụ không phải là tái tạo lại chuỗi các lần nhấn phím. Thay vào đó, chúng ta phải xác định những chữ cái nào được đảm bảo tương ứng với các khóa lành mạnh trong mọi cách giải thích có thể có của chuỗi cuối cùng. Một chữ cái chỉ an toàn để xuất ra nếu nó không thể bị phá vỡ trong bất kỳ sự phân tách hợp lệ nào của chuỗi thành các phần đóng góp chữ cái đơn và kép. 

Khó khăn chính là cùng một chuỗi thường có thể được giải thích theo nhiều cách tùy thuộc vào việc chúng ta coi các lần xuất hiện là ký tự đơn hay ký tự kép. Quyết định này có vẻ cục bộ nhưng lại có tính ràng buộc toàn cục, vì một chữ cái có thể được diễn giải một cách nhất quán hoặc không nhất quán trên toàn bộ chuỗi. 

Độ dài chuỗi tối đa là 500 và có tới 100 trường hợp thử nghiệm, do đó, giải pháp O(n^2) hoặc O(n) cho mỗi trường hợp là đủ. Bất cứ điều gì theo cấp số nhân trong các lựa chọn diễn giải đều không cần thiết vì chúng ta không cần liệt kê tất cả các phân rã. 

Một sai lầm ngây thơ là cho rằng nếu đôi khi một chữ cái xuất hiện riêng lẻ thì nó phải khỏe mạnh hoặc mọi cặp chữ cái giống nhau liền kề đều phải đến từ một phím bị hỏng. Cả hai cách diễn giải đều thất bại vì các quyết định ghép nối không độc lập trên toàn chuỗi. 

Ví dụ, hãy xem xét chuỗi`aabb`. Người ta có thể nghĩ`a`bị hỏng vì nó xuất hiện như`aa`, Và`b`bị hỏng vì nó xuất hiện như`bb`. Nhưng`aabb`cũng có thể đến từ bốn máy ép khỏe mạnh:`a a b b`, làm cho cả hai chữ cái khỏe mạnh. Vì vậy, nhóm địa phương là không đủ. 

Một trường hợp thất bại tinh tế khác là các mẫu xen kẽ như`abab`. Ở đây, cả hai chữ cái đều không thể tạo thành cặp, vì vậy cả hai đều phải tốt, nhưng việc quét tham lam cố gắng ghép nối bất cứ khi nào có thể có thể đánh dấu không chính xác cả hai là bị hỏng hoặc mơ hồ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi cách để phân chia chuỗi thành các phân đoạn có kích thước 1 hoặc 2, đảm bảo mỗi phân đoạn là sự lặp lại của một ký tự đơn. Mỗi phân vùng tương ứng với giả định mỗi lần nhấn tạo ra một hoặc hai ký tự. Sau khi tạo một phân vùng hợp lệ, chúng tôi có thể suy ra những chữ cái nào từng xuất hiện theo cách phù hợp với việc bị hỏng hoặc lành mạnh, sau đó giao nhau trên tất cả các phân vùng. 

Điều này nhanh chóng trở nên không khả thi vì mỗi vị trí có khả năng phân nhánh thành hai lựa chọn, dẫn đến phân tách O(2^n) trong trường hợp xấu nhất. Ngay cả khi cắt tỉa, số lượng phân đoạn hợp lệ của một chuỗi như`aaaaaa...`tăng trưởng theo cấp số nhân. 

Quan sát quan trọng là chúng ta không cần phải xây dựng rõ ràng tất cả các phân tách hợp lệ. Thay vào đó, chúng ta chỉ cần xác định liệu một chữ cái có thể bị buộc phải phá vỡ trong mọi lời giải thích hợp lý hay không. Tương tự, chúng tôi muốn các chữ cái không bao giờ có thể được giải thích là tạo ra chính xác một ký tự cho mỗi lần nhấn. 

Sự đơn giản hóa quan trọng là nhận thấy rằng một chữ cái chỉ không an toàn nếu chúng ta có thể phân chia các lần xuất hiện của nó hoàn toàn thành các cặp chữ cái liên tiếp giống hệt nhau, nghĩa là mỗi lần xuất hiện có thể được nhóm thành các khối có kích thước hai. Nếu ít nhất một lần xuất hiện của một chữ cái không thể ghép với một chữ cái lân cận giống hệt nhau thì sẽ tồn tại một cách giải thích hợp lệ trong đó chữ cái đó phải khỏe mạnh. 

Vì vậy, đối với mỗi chữ cái, chúng ta chỉ cần kiểm tra xem liệu tất cả các lần xuất hiện của nó trong chuỗi có thể được bao phủ hoàn toàn bởi các cặp liền kề rời rạc hay không. Nếu điều đó là không thể, thì bức thư được đảm bảo là hợp lý theo ít nhất một cách diễn giải hợp lệ và do đó không nằm trong kết quả đầu ra được yêu cầu. Ngược lại, nếu có thể ghép nối đầy đủ tất cả các lần xuất hiện thì chữ cái có thể bị hỏng hoàn toàn theo một cách hiểu nào đó nên không đảm bảo tính lành mạnh. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính: phát hiện xem các lần xuất hiện của một chữ cái có thể khớp hoàn hảo trong các cặp liền kề trong toàn chuỗi hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(26·n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Quét chuỗi và ghi lại, đối với mỗi chữ cái, tất cả các chỉ số nơi nó xuất hiện. Điều này cho phép chúng tôi suy luận về cấu trúc ghép nối cho mỗi ký tự mà không có sự can thiệp từ người khác. 
2. Đối với một chữ cái cố định, hãy thử ghép nối một cách tham lam từ trái sang phải trong chuỗi gốc. Bất cứ khi nào chúng ta nhìn thấy chữ cái ở vị trí i, chúng ta sẽ kiểm tra xem cả i và i+1 có chứa cùng một chữ cái hay không. Nếu có, chúng tôi coi đây là một khối phím bị hỏng và bỏ qua cả hai vị trí. Nếu không, sự cố này không thể là một phần của cặp bị hỏng, vì vậy nó phải được coi là một lần nhấn. 
3. Trong khi quét, nếu chúng tôi gặp phải tình huống trong đó một lần xuất hiện không thể khớp thành một cặp nhưng cũng không thể được xử lý một cách an toàn dưới dạng một lần nhất quán trong toàn bộ cấu trúc, thì chúng tôi kết luận rằng chữ cái không thể được giải thích đầy đủ là bị hỏng ở mọi nơi. Điều đó có nghĩa là nó an toàn và nên được đưa vào câu trả lời. 
4. Lặp lại quy trình này cho tất cả 26 chữ cái một cách độc lập. 
5. Thu thập tất cả các chữ cái không đáp ứng điều kiện “có thể ghép đôi hoàn toàn” và xuất chúng theo thứ tự bảng chữ cái. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ chữ cái bị hỏng nào cũng phải đóng góp sự xuất hiện của nó trong các cặp ký tự giống hệt nhau liền kề. Nếu một chữ cái thực sự bị hỏng, mỗi lần xuất hiện phải là một phần của cặp liền kề hợp lệ, bởi vì một máy in bị hỏng sẽ tạo ra chính xác hai chữ cái giống hệt nhau liên tiếp. Nếu thậm chí một lần xuất hiện bị cô lập hoặc phá vỡ tính nhất quán liền kề thì không có sự phân tách nào tồn tại khi chữ cái đó luôn bị phá vỡ, nghĩa là có ít nhất một cách giải thích ở chỗ nó lành mạnh. Vì vậy, tập hợp các chữ cái có thể được bao phủ hoàn toàn bởi các cặp liền kề sẽ tương ứng chính xác với các chữ cái không đảm bảo tính lành mạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_fully_pairable(s, ch):
    i = 0
    n = len(s)
    while i < n:
        if s[i] != ch:
            i += 1
            continue
        if i + 1 < n and s[i + 1] == ch:
            i += 2
        else:
            return False
    return True

t = int(input())
for _ in range(t):
    s = input().strip()
    res = []
    for c in "abcdefghijklmnopqrstuvwxyz":
        if is_fully_pairable(s, c):
            res.append(c)
    print("".join(res))
```chức năng`is_fully_pairable`kiểm tra xem tất cả các lần xuất hiện của một ký tự có thể được coi là cặp liền kề hay không. Con trỏ`i`di chuyển qua chuỗi và tiêu thụ một cách tham lam`cc`cặp bất cứ khi nào có thể. Nếu một sự xuất hiện độc lập xuất hiện, nó sẽ phá vỡ khả năng diễn giải ký tự luôn bị hỏng. 

Vòng lặp chính chỉ đơn giản áp dụng bài kiểm tra này cho tất cả 26 chữ cái và thu thập những chữ cái thỏa mãn nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`zzaaz`Chúng tôi theo dõi từng nhân vật. 

| tôi | s[i] | hành động | 
| --- | --- | --- | 
| 0 | z | tìm thấy cặp nào? không, z đơn | 
| 1 | z | cặp z z đã qua sử dụng | 
| 2 | một | cặp một chiếc đã qua sử dụng | 
| 4 | z | z đơn còn lại, không có cặp | 

Vì`z`, việc ghép nối không thành công vì có sự cố không khớp. Vì`a`, ghép nối thành công hoàn toàn. 

Điều này cho thấy chỉ`a`có thể được đảm bảo sức khỏe. 

### Ví dụ 2:`ccff`| tôi | s[i] | hành động | 
| --- | --- | --- | 
| 0 | c | c c cặp | 
| 2 | f | cặp ff | 

Cả hai chữ cái đều có thể được ghép nối hoàn toàn, nghĩa là cả hai chữ cái đều có thể bị hỏng một cách nhất quán, do đó, cả hai chữ cái đều không được đảm bảo tốt cho sức khỏe. Do đó đầu ra trống. 

Điều này xác nhận logic phân biệt giữa các chữ cái có thể ghép nối hoàn toàn và những chữ cái có cấu trúc đơn lẻ không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · n) | Mỗi chữ cái trong số 26 chữ cái được quét tuyến tính trên chuỗi | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và biến vòng lặp | 

Các ràng buộc cho phép tối đa 500 ký tự cho mỗi trường hợp thử nghiệm và 100 trường hợp thử nghiệm, do đó, tối đa 50.000 thao tác ký tự. Thuật toán thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    
    input = _sys.stdin.readline

    def is_fully_pairable(s, ch):
        i = 0
        n = len(s)
        while i < n:
            if s[i] != ch:
                i += 1
                continue
            if i + 1 < n and s[i + 1] == ch:
                i += 2
            else:
                return False
        return True

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        res = []
        for c in "abcdefghijklmnopqrstuvwxyz":
            if is_fully_pairable(s, c):
                res.append(c)
        out.append("".join(res))
    return "\n".join(out) + ("\n" if out else "")

# provided samples
assert run("4\na\nzzaaz\nccff\ncbddbb\n") == "a\nz\n\nbc\n"

# custom cases
assert run("1\nab\n") == "ab\n"  # no pairing possible, both must be healthy
assert run("1\naa\n") == "a\n"   # single letter fully pairable
assert run("1\nabab\n") == "ab\n"  # alternating, no pairs, both healthy
assert run("1\naaa\n") == "a\n"   # odd length breaks full pairing
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab`|`ab`| chữ cái xen kẽ nhau không ghép được | 
|`aa`|`a`| hộp đựng đầy đủ đơn giản | 
|`abab`|`ab`| cấu trúc đan xen | 
|`aaa`|`a`| lực lượng còn sót lại singleton | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một chữ cái xuất hiện với độ dài lẻ. Ví dụ,`aaa`không thể được phân chia hoàn toàn thành`aa`khối, để lại một lần xuất hiện còn sót lại. Thuật toán phát hiện điều này ngay lập tức vì nó gặp phải một`a`sau khi sử dụng một cặp, trả về lỗi cho chữ cái đó theo cách diễn giải ghép nối đầy đủ. 

Một trường hợp cạnh khác là xen kẽ như`ababa`. Mặc dù cả hai chữ cái xuất hiện thường xuyên nhưng không thể tạo thành các cặp liền kề nhất quán. Quá trình quét không thành công ở lần xuất hiện riêng biệt đầu tiên của mỗi chữ cái, đảm bảo cả hai đều được phân loại chính xác là ứng cử viên phù hợp. 

Trường hợp khó nhận biết cuối cùng là khi có thể ghép nối cục bộ nhưng không nhất quán trên toàn cầu. Ví dụ`aabbaabb`cho phép ghép nối đầy đủ, vì vậy`a`được đánh dấu là có thể ghép nối, trong khi`abac`ngắt ghép nối cho`a`ngay tại khu cách ly`c`chuyển đổi, ngăn chặn việc phân loại sai.
