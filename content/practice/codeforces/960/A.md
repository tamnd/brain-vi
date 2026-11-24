---
title: "CF 960A - Kiểm tra chuỗi"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự a, b và c. Chuỗi này được cho là đã được xây dựng theo một cách rất cụ thể: nó bắt đầu bằng một hoặc nhiều ký tự a, sau đó một số ký tự b được thêm vào và cuối cùng một số ký tự c được thêm vào cuối."
date: "2026-06-17T01:50:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 960
codeforces_index: "A"
codeforces_contest_name: "Divide by Zero 2018 and Codeforces Round 474 (Div. 1 + Div. 2, combined)"
rating: 1200
weight: 960
solve_time_s: 64
verified: true
draft: false
---

[CF 960A - Kiểm tra chuỗi](https://codeforces.com/problemset/problem/960/A) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm các ký tự`a`,`b`, Và`c`. Chuỗi này được cho là đã được xây dựng theo một cách rất cụ thể: nó bắt đầu bằng một hoặc nhiều`a`ký tự, sau đó một số`b`các ký tự được thêm vào và cuối cùng là một số`c`các ký tự được thêm vào cuối. Sau ba giai đoạn này, chuỗi phải chứa ít nhất một`a`và ít nhất một`b`, trong khi`c`có thể tồn tại hoặc không tùy theo quy luật xây dựng. 

Ràng buộc chính không chỉ về số lượng mà còn về cấu trúc. Chuỗi phải ở thứ tự pha không giảm: tất cả`a`đến trước, sau đó là tất cả`b`, thì tất cả`c`. Trên hết, số lượng`c`ký tự phải bằng số lượng ký tự`a`ký tự hoặc số lượng`b`ký tự (hoặc cả hai nếu chúng bằng nhau). 

Nhiệm vụ là quyết định xem một chuỗi nhất định có thể được tạo ra bởi một quy trình như vậy hay không. 

Kích thước đầu vào tối đa là 5000 ký tự. Điều này đủ nhỏ để bất kỳ giải pháp nào cho đến hành vi gần đúng bậc hai đều có thể vượt qua một cách thoải mái, nhưng cấu trúc của bài toán cho thấy quét tuyến tính là đủ. Vì chúng tôi chỉ đang xác minh một mẫu và không xây dựng bất kỳ thứ gì nên chúng tôi sẽ mong đợi kiểm tra O(n). 

Một sự hiểu lầm ngây thơ là chỉ tập trung vào số lượng. Ví dụ: một chuỗi như`acbbc`có số lượng bằng nhau`a`,`b`, Và`c`nhưng không hợp lệ vì đơn hàng bị hỏng. Một lỗi nhỏ khác là giả định rằng chuỗi có thể được sắp xếp lại nhưng việc sắp xếp lại là không được phép. 

Các trường hợp Edge thường phá vỡ các giải pháp không chính xác bao gồm: 

- Thứ tự hỗn hợp như`aba...`Ở đâu`b`xuất hiện trước tất cả`a`đã xong. 
- Thiếu các phân đoạn bắt buộc, chẳng hạn như không`b`không hề. 
- Thứ tự đúng nhưng sai`c`đếm, chẳng hạn như`aabccc`Ở đâu`a=2`,`b=1`,`c=3`, không hợp lệ vì`c`không khớp với 2 hay 1. 
- Chuỗi như`aaabbbccc`nơi mọi thứ đều sạch sẽ và đếm trận đấu`a`hoặc`b`. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử chia chuỗi thành ba đoạn và kiểm tra tất cả các điểm phân chia có thể có. Đối với mỗi lần chia`(i, j)`, chúng tôi giải thích`S[0:i]`như tất cả`a`,`S[i:j]`như tất cả`b`, Và`S[j:]`như tất cả`c`, sau đó xác minh cả thứ tự và điều kiện đếm. Điều này đòi hỏi phải thử phân tách O(n²) và mỗi lần xác minh tốn O(n), dẫn đến O(n³) trong trường hợp xấu nhất, điều này không cần thiết và lãng phí ngay cả đối với n = 5000. 

Cấu trúc của bài toán loại bỏ nhu cầu đoán các điểm phân chia. Chuỗi, nếu hợp lệ, có mẫu đơn điệu cứng nhắc: một khi loại ký tự thay đổi, nó sẽ không bao giờ trả về. Điều này có nghĩa là chúng tôi có thể quét từ trái sang phải và đảm bảo quá trình chuyển đổi diễn ra theo đúng`a → b → c`chỉ một. Đồng thời, chúng tôi đếm số lần xuất hiện của từng ký tự. 

Sau khi quá trình quét kết thúc, chúng ta chỉ cần kiểm tra hai điều kiện: thỏa mãn ràng buộc thứ tự và thỏa mãn số đếm cuối cùng`c == a or c == b`, với yêu cầu ngầm bổ sung là cả hai`a > 0`Và`b > 0`bởi vì việc xây dựng đảm bảo ít nhất một trong số đó. 

Điều này làm giảm toàn bộ vấn đề xuống còn một lần duy nhất với việc ghi sổ liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force chia tách | O(n³) | O(1) | Quá chậm | 
| Đếm một lần quét | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`ca`,`cb`,`cc`về không. Đồng thời theo dõi một biến giai đoạn đại diện cho khối mà chúng ta hiện đang ở:`0`vì`a`,`1`vì`b`,`2`vì`c`. 
2. Quét chuỗi từ trái sang phải. 
3. Với mỗi nhân vật: 

- Nếu vậy`a`, nó chỉ hợp lệ nếu chúng ta vẫn đang trong giai đoạn`0`. Tăng`ca`. 
- Nếu vậy`b`, chúng ta phải ở trong giai đoạn`0`hoặc`1`. Nếu chúng ta đang ở trong giai đoạn`0`, chuyển sang sân khấu`1`. Tăng`cb`. 
- Nếu vậy`c`, chúng ta phải ở trong giai đoạn`1`hoặc`2`. Nếu chúng ta đang ở trong giai đoạn`1`, chuyển sang sân khấu`2`. Tăng`cc`. 

Bất kỳ vi phạm nào về chuyển đổi được phép sẽ ngay lập tức làm mất hiệu lực chuỗi. 
4. Sau khi quét, đảm bảo rằng cả hai`ca > 0`Và`cb > 0`. 
5. Cuối cùng kiểm tra xem`cc == ca`hoặc`cc == cb`. 

Lý do thực thi quá trình chuyển đổi trong quá trình quét là để đảm bảo chúng tôi không bao giờ cho phép`b`trước khi kết thúc tất cả`a`hoặc một`c`trước khi kết thúc tất cả`b`. 

### Tại sao nó hoạt động 

Quá trình xây dựng đảm bảo cấu trúc ba pha đơn điệu. Bất kỳ chuỗi hợp lệ nào cũng phải có một phân vùng duy nhất thành các khối liền kề nhau.`a`, sau đó`b`, sau đó`c`. Biến giai đoạn thực thi chính xác việc phân vùng này. Vì mỗi ký tự được xử lý một lần và chỉ một lần theo các quy tắc chuyển tiếp nghiêm ngặt nên mọi sai lệch so với thứ tự yêu cầu sẽ được phát hiện ngay lập tức. Sau khi cấu trúc được xác minh, mức độ tự do duy nhất còn lại là số lượng`c`, phải khớp với một trong các kích thước khối trước đó theo định nghĩa của công trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

ca = cb = cc = 0
stage = 0  # 0 = a, 1 = b, 2 = c

valid = True

for ch in s:
    if ch == 'a':
        if stage != 0:
            valid = False
            break
        ca += 1
    elif ch == 'b':
        if stage == 2:
            valid = False
            break
        if stage == 0:
            stage = 1
        cb += 1
    else:  # 'c'
        if stage < 1:
            valid = False
            break
        if stage == 1:
            stage = 2
        cc += 1

if not valid or ca == 0 or cb == 0:
    print("NO")
else:
    print("YES" if (cc == ca or cc == cb) else "NO")
```Mã duy trì một biến pha nghiêm ngặt`stage`mã hóa phân đoạn nào chúng tôi hiện được phép mở rộng. Điều này ngăn chặn việc chuyển đổi ngược bất hợp pháp chẳng hạn như gặp phải một`a`sau một`b`. Bộ đếm theo dõi tần số chính xác cần thiết cho điều kiện cuối cùng. 

Một điểm tinh tế là quá trình chuyển đổi chỉ tiến về phía trước: một khi chúng ta bước vào giai đoạn`1`, chúng tôi không bao giờ quay trở lại. Điều này phù hợp với tính không thể đảo ngược của quá trình xây dựng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`aaabccc`| chỉ mục | char | giai đoạn trước | giai đoạn sau | ca | cb | cc | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 1 | 0 | 0 | vâng | 
| 2 | một | 0 | 0 | 2 | 0 | 0 | vâng | 
| 3 | một | 0 | 0 | 3 | 0 | 0 | vâng | 
| 4 | b | 0 | 1 | 3 | 1 | 0 | vâng | 
| 5 | c | 1 | 2 | 3 | 1 | 1 | vâng | 
| 6 | c | 2 | 2 | 3 | 1 | 2 | vâng | 
| 7 | c | 2 | 2 | 3 | 1 | 3 | vâng | 

Kiểm tra lần cuối:`ca = 3`,`cb = 1`,`cc = 3`. Từ`cc == ca`, đầu ra là`YES`. 

Dấu vết này xác nhận rằng một khi chuyển sang`b`và sau đó`c`xảy ra, việc khóa giai đoạn sẽ duy trì trật tự một cách chính xác. 

### Ví dụ 2:`aabcb`| chỉ mục | char | giai đoạn trước | giai đoạn sau | ca | cb | cc | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 1 | 0 | 0 | vâng | 
| 2 | một | 0 | 0 | 2 | 0 | 0 | vâng | 
| 3 | b | 0 | 1 | 2 | 1 | 0 | vâng | 
| 4 | c | 1 | 2 | 2 | 1 | 1 | vâng | 
| 5 | b | 2 | không hợp lệ | 2 | 1 | 1 | không | 

Khoảnh khắc chúng ta nhìn thấy`b`sau khi vào`c`vi phạm nội quy xây dựng. Điều này chứng tỏ tại sao việc thực thi lệnh là cần thiết, vì chỉ riêng việc kiểm tra số lượng sẽ chấp nhận nó một cách không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong một lần chuyển | 
| Không gian | O(1) | Chỉ sử dụng một số bộ đếm cố định và biến giai đoạn | 

Giới hạn đầu vào 5000 ký tự có thể được xử lý dễ dàng bằng quét tuyến tính, thực hiện tối đa 5000 lần lặp và công việc liên tục trên mỗi lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from sys import stdout
    input = _sys.stdin.readline

    s = input().strip()

    ca = cb = cc = 0
    stage = 0
    valid = True

    for ch in s:
        if ch == 'a':
            if stage != 0:
                valid = False
                break
            ca += 1
        elif ch == 'b':
            if stage == 2:
                valid = False
                break
            if stage == 0:
                stage = 1
            cb += 1
        else:
            if stage < 1:
                valid = False
                break
            if stage == 1:
                stage = 2
            cc += 1

    if not valid or ca == 0 or cb == 0:
        return "NO"
    return "YES" if (cc == ca or cc == cb) else "NO"

# provided sample
assert run("aaabccc\n") == "YES"

# custom cases
assert run("aaaabbbcccc\n") == "YES"
assert run("aaaa\n") == "NO"
assert run("ab\n") == "NO"
assert run("aabbccc\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaaa`| KHÔNG | mất tích`b`Và`c`yêu cầu pha | 
|`ab`| KHÔNG | mất tích`c`và không hợp lệ`c`bình đẳng | 
|`aabbccc`| KHÔNG |`c`đếm điều kiện không phù hợp | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi chuỗi có thứ tự đúng nhưng không đáp ứng được ràng buộc về số lượng. Đối với đầu vào`aaabbbcccc`, quá trình quét tạo ra`ca=3`,`cb=3`,`cc=4`. Logic giai đoạn chấp nhận nó về mặt cấu trúc, nhưng kiểm tra cuối cùng sẽ từ chối nó vì`cc`không khớp`ca`cũng không`cb`. 

Một trường hợp khác là khi quá trình chuyển đổi xảy ra quá sớm. Vì`baaa`, ký tự đầu tiên`b`giai đoạn lực lượng`1`, nhưng gặp phải`a`sau đó vi phạm quy tắc ngay lập tức, gây ra sự từ chối sớm trước bất kỳ vấn đề đếm nào. 

Trường hợp cuối cùng là cấu trúc hợp lệ tối thiểu như`abc`. Bộ quét`ca=1`,`cb=1`,`cc=1`, và kể từ đó`cc == ca == cb`, nó được chấp nhận, xác nhận rằng sự bằng nhau của cả ba phân đoạn được xử lý một cách tự nhiên bởi điều kiện cuối cùng.
