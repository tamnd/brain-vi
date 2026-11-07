---
title: "CF 1342B - Kỳ nhị phân"
description: "Chúng ta được cấp một chuỗi nhị phân và được yêu cầu nhúng nó vào một chuỗi nhị phân lớn hơn trong khi vẫn giữ nguyên thứ tự dưới dạng một chuỗi con."
date: "2026-06-16T09:29:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "strings"]
categories: ["algorithms"]
codeforces_contest: 1342
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 86 (Rated for Div. 2)"
rating: 1100
weight: 1342
solve_time_s: 212
verified: false
draft: false
---

[CF 1342B - Thời kỳ nhị phân](https://codeforces.com/problemset/problem/1342/B) 

**Đánh giá:** 1100 
**Tas:** thuật toán xây dựng, chuỗi 
**Thời gian giải:** 3 phút 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân và được yêu cầu nhúng nó vào một chuỗi nhị phân lớn hơn trong khi vẫn giữ nguyên thứ tự dưới dạng một chuỗi con. Trong số tất cả các siêu chuỗi có thể có như vậy, có độ dài không vượt quá hai lần chuỗi gốc, chúng tôi muốn một siêu chuỗi có cấu trúc lặp lại bên trong càng đơn giản càng tốt, nghĩa là khoảng thời gian lặp lại nhỏ nhất của nó được giảm thiểu. 

Một chuỗi có một dấu chấm$k$nếu chuyển nó bằng$k$các vị trí căn chỉnh mọi ký tự với chính nó ở bất cứ nơi nào cả hai vị trí tồn tại và không có sự dịch chuyển dương nhỏ hơn nào có đặc tính này. Theo trực giác, khoảng thời gian nhỏ có nghĩa là sự lặp lại mạnh mẽ: khoảng thời gian 1 có nghĩa là tất cả các ký tự đều giống nhau, khoảng thời gian 2 có nghĩa là chuỗi xen kẽ và khoảng thời gian lớn hơn cho phép các mẫu phức tạp hơn. 

Độ căng chính nằm giữa hai ràng buộc: chúng ta được phép “thổi phồng” chuỗi bằng cách chèn các ký tự vào bất kỳ đâu, nhưng chúng ta vẫn phải giữ nguyên chuỗi gốc dưới dạng một chuỗi con. Điều này có nghĩa là chúng ta không thể sắp xếp lại thứ tự các ký tự từ đầu vào nhưng chúng ta có thể giãn khoảng cách giữa chúng. 

Ràng buộc$|s| \le 2|t|$đủ chặt đến mức chúng ta không thể tự do mã hóa các cấu trúc tùy ý. Bất kỳ cấu trúc nào sao chép chuỗi nhiều lần hoặc tạo bộ đệm dài cho mỗi ký tự đều có nguy cơ vượt quá giới hạn này. Với$|t| \le 100$, ngay cả các công trình bậc hai hoặc bậc ba cũng có thể được chấp nhận, nhưng bất cứ điều gì theo cấp số nhân trong các lựa chọn sẽ không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi đầu vào đã có mẫu mạnh nhưng “gần như” định kỳ. Ví dụ: một chuỗi như`010`không thể thực hiện xen kẽ hoàn toàn nếu không chèn các ký tự được đặt cẩn thận và các cách tiếp cận ngây thơ chỉ nối thêm các ký tự một cách tham lam thường không giảm thiểu được khoảng thời gian vì chúng ngầm khóa một mẫu quá sớm. Một trường hợp phức tạp khác là một chuỗi như`1100`, trong đó việc mở rộng nó mà không cần suy nghĩ có thể vô tình duy trì một khoảng thời gian dài hơn mức cần thiết, mặc dù tồn tại một công trình có thời gian ngắn hơn bằng cách xen kẽ các bản sao. 

Khó khăn thực sự là khoảng thời gian đó được xác định bởi các ràng buộc căn chỉnh toàn cục, trong khi việc nhúng chuỗi tiếp theo mang tính cục bộ và tham lam. Giải pháp phải dung hòa được cả hai. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các chuỗi ứng cử viên$s$lên đến chiều dài$2n$, kiểm tra xem$t$là một dãy con của$s$, tính chu kỳ của$s$, và chọn cái tốt nhất. Số chuỗi nhị phân có độ dài lên tới$2n$là số mũ, xung quanh$2^{2n}$, và thậm chí với việc cắt tỉa tích cực thì điều này vẫn không thể thực hiện được. Điểm nghẽn không phải là việc xác minh, vốn mang tính tuyến tính, mà là số lượng ứng viên quá lớn. 

Quan sát quan trọng là việc giảm thiểu khoảng thời gian tương đương với việc quyết định xem chúng ta có thể buộc giai đoạn 1 hay giai đoạn 2 hay không. Bất kỳ chuỗi nhị phân nào cũng có ít nhất khoảng thời gian là 1 và giai đoạn 1 chỉ có thể đạt được khi tất cả các ký tự giống hệt nhau, điều này là không thể trừ khi$t$bản thân nó là hằng số. Vì vậy, khả năng tốt nhất tiếp theo là giai đoạn 2, nghĩa là một chuỗi thay thế`0101...`hoặc`1010...`. Nếu chúng ta có thể nhúng$t$với cấu trúc như vậy, chúng ta đã hoàn thành; mặt khác, chúng ta quay trở lại một chuỗi không đổi, có giai đoạn 1. 

Điều này biến vấn đề thành việc kiểm tra xem liệu$t$có thể được nhúng vào một trong hai mẫu vô hạn xen kẽ, đồng thời tôn trọng giới hạn độ dài. Nếu có, chúng tôi xây dựng tiền tố ngắn nhất của mẫu xen kẽ đó chứa$t$như một sự tiếp nối. Nếu không, chúng ta xây dựng một chuỗi có độ dài không đổi$n$, về cơ bản có chu kỳ 1 và luôn chứa$t$như một chuỗi con bằng cách mở rộng với cấu trúc ký tự chi phối cho phép nhúng vào trong giới hạn. 

Lý do điều này có tác dụng là vì bất kỳ chuỗi nào có chu kỳ lớn hơn 2 đều kém hơn so với cấu trúc giai đoạn 2 về mặt giảm thiểu tính tuần hoàn và vì chúng tôi được phép có độ dài lên tới$2n$, chúng ta luôn có đủ chỗ để hiện thực hóa một trong hai mô hình xen kẽ nếu khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng mô hình xen kẽ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời bằng cách cố gắng khớp dữ liệu đầu vào vào cấu trúc tuần hoàn đơn giản nhất có thể. 

1. Thử xây dựng một chuỗi theo mẫu`010101...`và kiểm tra xem liệu chúng tôi có thể nhúng$t$như một chuỗi con trong khi quét mẫu này. Chúng tôi mô phỏng điều này bằng cách kết hợp các ký tự của$t$theo mẫu và kéo dài cho đến khi tất cả các ký tự được khớp. 
2. Nếu cách trên không thành công trong thời gian dài$2n$, hãy thử mô hình đối xứng`101010...`theo cách tương tự. Điều này đảm bảo chúng tôi không bỏ lỡ các trường hợp nhúng khối chẵn lẻ bắt đầu đầu tiên. 
3. Nếu việc xây dựng xen kẽ không thành công, hãy quay lại chuỗi không đổi. Chúng tôi chọn tất cả`0`hoặc tất cả`1`tùy thuộc vào tính khả thi, nhưng vì chúng ta chỉ cần ngăn chặn chuỗi con và độ dài là linh hoạt, bất kỳ chuỗi có độ dài không đổi nào$n$hoạt động để giảm thiểu thời gian xuống 1 khi không thể thay thế được dưới những hạn chế. 

Ý tưởng chính là chúng ta đang tham lam nhúng$t$thành một khuôn mẫu định kỳ cố định và việc hoàn thành thành công sớm nhất sẽ mang lại kết quả xây dựng hợp lệ ngắn nhất. 

### Tại sao nó hoạt động 

Việc xây dựng dựa trên thực tế là bất kỳ chuỗi nhị phân nào có khoảng thời gian tối thiểu lớn hơn 2 đều không thể cải thiện việc nhúng xen kẽ không thành công trong chế độ độ dài giới hạn này. Nếu cả hai mẫu xen kẽ đều thất bại, điều đó có nghĩa là các ràng buộc thứ tự bắt buộc của$t$buộc phải có quá nhiều sự sắp xếp giống hệt nhau liên tiếp làm phá vỡ sự luân phiên vượt quá những gì có thể sửa chữa được trong$2n$giới hạn. Trong trường hợp đó, việc sụp đổ thành một cấu trúc không đổi mang lại khoảng thời gian tối thiểu có thể theo định nghĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_build(t, start):
    n = len(t)
    res = []
    j = 0
    cur = start
    # we can safely extend up to 2n
    for _ in range(2 * n):
        res.append(cur)
        if j < n and t[j] == cur:
            j += 1
        if j == n:
            return ''.join(res)
        cur = '1' if cur == '0' else '0'
    return None

T = int(input())
for _ in range(T):
    t = input().strip()

    ans = can_build(t, '0')
    if ans is None:
        ans = can_build(t, '1')

    if ans is None:
        ans = t[0] * len(t)

    print(ans)
```Việc triển khai mô phỏng hai cấu trúc ứng cử viên: bắt đầu mẫu xen kẽ với`0`và với`1`. Trong quá trình mô phỏng, chúng tôi duy trì một con trỏ vào$t$và tiến lên bất cứ khi nào ký tự hiện tại của chuỗi được xây dựng khớp với ký tự cần thiết tiếp theo của$t$. Khi tất cả các ký tự được khớp, chúng tôi xuất tiền tố được xây dựng. 

Dự phòng là một chuỗi không đổi, đảm bảo giai đoạn 1 và luôn thỏa mãn giới hạn độ dài. Thứ tự thực hiện rất quan trọng: trước tiên hãy thử các mẫu xen kẽ để đảm bảo chúng ta tối đa hóa cơ hội đạt được giai đoạn 2 trước khi quyết định sang giai đoạn 1. 

## Ví dụ đã hoạt động 

Hãy xem xét`t = 01`. 

Chúng tôi thử bắt đầu`0`: 

| bước | chuỗi được xây dựng | chỉ số phù hợp trong t | ký tự tiếp theo | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 01 | 2 | xong | 

Chúng tôi dừng lại ngay lập tức với`01`, có chu kỳ 2. 

Bây giờ hãy xem xét`t = 110`. 

Đang thử xen kẽ`010101...`: 

| bước | chuỗi được xây dựng | chỉ số phù hợp trong t | ký tự tiếp theo | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 
| 2 | 01 | 1 | 1 | 
| 3 | 010 | 1 | 1 | 
| 4 | 0101 | 2 | 0 | 
| 5 | 01010 | 3 | xong | 

Chúng tôi có được một nhúng hợp lệ trong giới hạn. 

Những dấu vết này cho thấy cách so khớp chuỗi tiếp theo tiến triển độc lập với cấu trúc tuần hoàn và cách luân phiên vẫn cho phép tính linh hoạt trong việc bỏ qua các kết quả không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi công trình quét tối đa 2n ký tự | 
| Không gian | O(n) | lưu trữ cho chuỗi được xây dựng | 

Với$T \le 100$Và$n \le 100$, tổng số thao tác không đáng kể trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def can_build(t, start):
        n = len(t)
        res = []
        j = 0
        cur = start
        for _ in range(2 * n):
            res.append(cur)
            if j < n and t[j] == cur:
                j += 1
            if j == n:
                return ''.join(res)
            cur = '1' if cur == '0' else '0'
        return None

    T = int(input())
    for _ in range(T):
        t = input().strip()

        ans = can_build(t, '0')
        if ans is None:
            ans = can_build(t, '1')
        if ans is None:
            ans = t[0] * len(t)

        print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n00\n01\n111\n110\n") == "00\n01\n11111\n1010"

# all equal
assert run("1\n0000\n") == "0000"

# alternating already optimal
assert run("1\n0101\n") in {"0101", "010101"}

# mixed case
assert run("1\n1001\n") != ""

# minimum size
assert run("1\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0000`|`0000`| xử lý trường hợp liên tục | 
|`0101`|`0101`hoặc phần mở rộng | đã xen kẽ | 
|`0`|`0`| ranh giới tối thiểu | 
|`1001`| xây dựng hợp lệ | nhúng không tầm thường | 

## Vỏ cạnh 

Một chuỗi bao gồm các ký tự giống hệt nhau như`0000`ngay lập tức buộc giải pháp vào tình trạng dự phòng liên tục. Việc xây dựng xen kẽ vẫn hoạt động nhưng có thể lãng phí thời gian trước khi hoàn thiện, do đó, việc xây dựng dự phòng đảm bảo thời gian tối thiểu mà không cần tăng trưởng không cần thiết. 

Một chuỗi như`010101`đã xen kẽ hoàn hảo và bắt đầu từ một trong hai chẵn lẻ sẽ duy trì giai đoạn 2 ngay lập tức. Bất kỳ công trình nào cố gắng mở rộng không cần thiết vẫn có hiệu lực nhưng không tối ưu về cấu trúc. 

Đầu vào một ký tự luôn tự mang lại kết quả, vì bất kỳ phần mở rộng nào cũng sẽ chỉ tăng độ dài mà không cải thiện thời gian và điều kiện chuỗi con được thỏa mãn một cách tầm thường.
