---
title: "CF 1833A - Câu đố âm nhạc"
description: "Chúng ta được cung cấp một giai điệu mục tiêu, giai điệu này chỉ là một chuỗi trên bảng chữ cái {a, b, c, d, e, f, g}. Vlad không thể trực tiếp xây dựng chuỗi này theo từng đoạn tùy ý. Thay vào đó, anh ta chỉ có thể ghi lại các đoạn có độ dài chính xác bằng hai ký tự, như \"ab\" hoặc \"gg\"."
date: "2026-06-09T06:55:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1833
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 874 (Div. 3)"
rating: 800
weight: 1833
solve_time_s: 91
verified: true
draft: false
---

[CF 1833A - Câu đố âm nhạc](https://codeforces.com/problemset/problem/1833/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một giai điệu mục tiêu, đó chỉ là một chuỗi trên bảng chữ cái`{a, b, c, d, e, f, g}`. Vlad không thể trực tiếp xây dựng chuỗi này theo từng đoạn tùy ý. Thay vào đó, anh ta chỉ có thể ghi lại những đoạn có độ dài đúng hai ký tự, như`"ab"`hoặc`"gg"`. 

Sau khi ghi lại một số mảnh có hai ký tự như vậy, anh ta được phép ghép chúng thành một chuỗi dài hơn, nhưng chỉ khi các mảnh liền kề “khớp” ở ranh giới. Cụ thể, ký tự cuối cùng của mảnh này phải bằng ký tự đầu tiên của mảnh tiếp theo, để việc ghép không tạo ra bước nhảy trái phép. 

Mục tiêu là xây dựng lại toàn bộ chuỗi đã cho bằng cách sử dụng càng ít bản ghi hai ký tự càng tốt. 

Một cách hữu ích để diễn giải lại vấn đề là coi mỗi bản ghi hai ký tự là một kết nối trực tiếp giữa ký tự đầu tiên và ký tự thứ hai của nó. Khi chúng tôi ghép nối các bản ghi, về cơ bản chúng tôi đang thực hiện dọc theo các kết nối này, sử dụng lại điều kiện liên tục của điểm cuối làm hạn chế đi bộ. Nhiệm vụ trở thành: cần bao nhiêu cạnh được định hướng như vậy để sự chồng chéo của chúng có thể tạo ra toàn bộ chuỗi. 

Độ dài chuỗi tối đa là 50 và có tối đa 10⁴ trường hợp thử nghiệm. Điều đó có nghĩa là bất kỳ giải pháp O(n²) hoặc tốt hơn nào trên mỗi thử nghiệm đều dễ dàng đủ nhanh, nhưng bất kỳ điều gì theo cấp số nhân hoặc liên quan đến tìm kiếm nhiều trên các tập hợp con của các cạnh sẽ là không cần thiết và rủi ro. 

Một trường hợp tinh tế phát sinh khi tất cả các ký tự trong chuỗi giống hệt nhau, chẳng hạn như`"aaaaaa"`. Đây là một bản ghi duy nhất`"aa"`về mặt khái niệm có thể được sử dụng lại để mở rộng toàn bộ chuỗi, vì vậy câu trả lời trở thành 1. Một trường hợp đặc biệt khác là khi tất cả các chuyển đổi đều khác biệt theo mô hình zig-zag như`"abacaba"`, trong đó việc sử dụng lại bị hạn chế và chúng tôi phải tính đến các chuyển tiếp lặp lại giữa các cặp chồng chéo. 

Một sai lầm ngây thơ là cho rằng chúng ta cần một bản ghi cho mỗi cặp liền kề trong chuỗi. Điều này không thành công vì một cặp được ghi có thể được sử dụng lại nhiều lần trong chuỗi nếu điểm cuối của nó khớp phù hợp, làm giảm số lượng bản ghi cần thiết. 

## Phương pháp tiếp cận 

Cách diễn giải brute-force cố gắng chọn rõ ràng những phân đoạn hai ký tự nào cần ghi lại và cách ghép chúng lại với nhau để tạo thành chuỗi. Người ta có thể tưởng tượng việc liệt kê tất cả các lựa chọn nhiều cạnh có thể có của các cạnh như`"ab"`,`"ba"`,`"ac"`, v.v., sau đó kiểm tra xem chuỗi có thể được tạo thành bằng cách xâu chuỗi chúng theo thứ tự hay không. Điều này nhanh chóng trở nên khó giải quyết vì mặc dù bảng chữ cái nhỏ nhưng số lượng các cạnh có thể có sẽ tăng theo cấp số nhân với số lần chuyển tiếp riêng biệt trong chuỗi. Mỗi lần kiểm tra sẽ yêu cầu mô phỏng việc xây dựng đường dẫn, khiến cách tiếp cận này quá chậm ngay cả với n = 50. 

Thông tin chi tiết quan trọng là ngừng suy nghĩ về các chuỗi đầy đủ đang được xây dựng và thay vào đó hãy xem xét các chuyển đổi cục bộ giữa các ký tự liên tiếp trong chuỗi mục tiêu. Mỗi cặp liền kề`(s[i], s[i+1])`yêu cầu một số phân đoạn được ghi có thể bao gồm nó, nghĩa là chúng tôi cần đảm bảo rằng mọi loại chuyển tiếp được định hướng đều có sẵn ít nhất một lần. Tuy nhiên, vấn đề chồng chéo là: nếu chúng ta đã có một phân khúc`"ab"`và sau này cần`"ab"`Một lần nữa, chúng tôi không cần bản ghi thứ hai miễn là nó có thể được sử dụng lại trong quá trình xây dựng. 

Sự đơn giản hóa quan trọng là cấu trúc của chuỗi chỉ buộc chúng ta phải quan tâm đến sự chuyển tiếp riêng biệt giữa các ký tự, nhưng với một ngoại lệ: khi một ký tự xuất hiện trong một mẫu mà nó phải đóng vai trò là điểm phân nhánh, nó có thể buộc phải ghi thêm vì chỉ một cạnh không thể phục vụ nhiều hướng không tương thích trong một chuỗi. Trong thực tế, giải pháp tối ưu giảm thiểu việc đếm số lần chuyển đổi giữa các ký tự khác nhau là “mới” trong quá trình quét tham lam, đồng thời sử dụng lại các chuyển đổi đã thấy trước đó bất cứ khi nào có thể. 

Công thức đúng sẽ trở thành: chúng ta mô phỏng một cách tham lam việc xây dựng chuỗi và đếm xem chúng ta phải “bắt đầu một bản ghi mới” bao nhiêu lần vì quá trình chuyển đổi hiện tại không thể được thỏa mãn bằng cách tiếp tục chuỗi trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên nhiều cạnh | Hàm mũ | O(n) | Quá chậm | 
| Theo dõi quá trình chuyển đổi tham lam | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập. 

1. Bắt đầu với một tập hợp trống các chuyển tiếp có hướng đã sử dụng. Mỗi quá trình chuyển đổi là một cặp`(s[i], s[i+1])`. 
2. Quét chuỗi từ trái sang phải và xem xét từng cặp liền kề. Đối với mỗi cặp, hãy kiểm tra xem chúng tôi đã “bao gồm” quá trình chuyển đổi này trong bộ bản ghi của mình chưa. Nếu không, chúng tôi phải giới thiệu bản ghi hai ký tự mới cho cặp này. 
3. Khi chúng tôi giới thiệu bản ghi âm mới cho một cặp`(a, b)`, chúng tôi đánh dấu nó là đã được sử dụng. Điều này thể hiện việc ghi lại phân đoạn`"ab"`. 
4. Tiếp tục quét. Nếu sau này chúng ta gặp phải cặp đôi tương tự`(a, b)`, chúng ta không cần bản ghi khác vì nó có thể sử dụng lại bản ghi hiện có trong chuỗi. 
5. Câu trả lời cuối cùng là số lần chuyển đổi có hướng riêng biệt xuất hiện trong chuỗi. 

Một chi tiết quan trọng là chúng tôi xử lý các chuyển đổi theo chỉ dẫn, nghĩa là`"ab"`Và`"ba"`là những bản ghi khác nhau. Điều này rất cần thiết vì ràng buộc nối phụ thuộc vào hướng. 

### Tại sao nó hoạt động 

Mỗi chuỗi hai ký tự được ghi tương ứng chính xác với một cạnh có hướng. Bất kỳ cấu trúc hợp lệ nào của chuỗi cuối cùng đều phải có khả năng nhận ra mọi cặp liền kề trong chuỗi ban đầu thông qua một số cạnh. Nếu hai quá trình chuyển đổi giống hệt nhau xuất hiện thì một cạnh được ghi lại là đủ vì nó có thể được sử dụng lại ở nhiều vị trí trong chuỗi miễn là duy trì được khả năng kết nối. Do đó, số lượng bản ghi tối thiểu bằng số cặp ký tự liền kề riêng biệt mà chuỗi yêu cầu. 

Điều bất biến là ở mỗi bước quét, tập hợp các chuyển tiếp được ghi lại đủ để tái tạo tất cả các cặp kề kề đã được xử lý mà không có mâu thuẫn và chúng tôi chỉ mở rộng nó khi một loại kề cận mới xuất hiện. Điều này đảm bảo tính tối thiểu vì việc giới thiệu ít cạnh hơn sẽ khiến ít nhất một cạnh kề không được hỗ trợ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    used = set()
    ans = 0

    for i in range(n - 1):
        pair = (s[i], s[i + 1])
        if pair not in used:
            used.add(pair)
            ans += 1

    print(ans)
```Việc triển khai phản ánh trực tiếp ý tưởng theo dõi các cặp liền kề riêng biệt. bộ`used`các cửa hàng chuyển đổi trực tiếp đã được tính toán. Mỗi lần chúng tôi nhìn thấy một cặp mới, chúng tôi sẽ tăng câu trả lời. Không có vấn đề ranh giới nào ngoài việc chỉ lặp lại tối đa`n-2`, vì chúng tôi luôn truy cập`i+1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abab
```Chúng tôi theo dõi các cặp liền kề: 

| tôi | Cặp | Đã thấy trước đây? | Bộ đã qua sử dụng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | (a,b) | không | {(a,b)} | 1 | 
| 1 | (b,a) | không | {(a,b),(b,a)} | 2 | 
| 2 | (a,b) | vâng | {(a,b),(b,a)} | 2 | 

Đầu ra là 2. 

Điều này thể hiện khả năng sử dụng lại: một khi quá trình chuyển đổi được ghi lại, việc lặp lại nó sẽ không làm tăng chi phí. 

### Ví dụ 2 

đầu vào:```
abacaba
```| tôi | Cặp | Đã thấy trước đây? | Bộ đã qua sử dụng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | (a,b) | không | {(a,b)} | 1 | 
| 1 | (b,a) | không | {(a,b),(b,a)} | 2 | 
| 2 | (a,c) | không | {(a,b),(b,a),(a,c)} | 3 | 
| 3 | (c,a) | không | {(a,b),(b,a),(a,c),(c,a)} | 4 | 
| 4 | (a,b) | vâng | giống nhau | 4 | 
| 5 | (b,a) | vâng | giống nhau | 4 | 

Đầu ra là 4. 

Điều này cho thấy việc phân nhánh ở một ký tự sẽ tạo ra nhiều chuyển đổi riêng biệt như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chúng tôi quét từng chuỗi một lần và thực hiện các thao tác tập hợp O(1) mỗi bước | 
| Không gian | O(1) | Tối đa 49 cặp riêng biệt cho mỗi bài kiểm tra kể từ n 50 | 

Các ràng buộc cho phép tối đa 10⁴ trường hợp thử nghiệm, nhưng tổng số ký tự được xử lý vẫn đủ nhỏ để phương pháp tiếp cận tuyến tính trên mỗi thử nghiệm này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        used = set()
        ans = 0
        for i in range(n - 1):
            p = (s[i], s[i+1])
            if p not in used:
                used.add(p)
                ans += 1
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
4
abab
7
abacaba
6
aaaaaa
7
abcdefg
5
babdd
""") == """2
4
1
6
4"""

# custom cases
assert run("""1
2
aa
""") == "1"

assert run("""1
3
abc
""") == "2"

assert run("""1
5
ababa
""") == "2"

assert run("""1
6
ababab
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"aa"`| 1 | trường hợp tự lặp đơn | 
|`"abc"`| 2 | chuỗi chuyển tiếp đơn giản khác biệt | 
|`"ababa"`| 2 | tái sử dụng theo mô hình xen kẽ | 
|`"ababab"`| 2 | cấu trúc xen kẽ lặp đi lặp lại | 

## Vỏ cạnh 

Đối với một chuỗi như`"aaaaaa"`, mọi cặp liền kề đều giống hệt nhau. Thuật toán nhìn thấy`(a,a)`một lần và không bao giờ tăng bộ đếm nữa, tạo ra đầu ra 1. Điều này phù hợp với thực tế là một bản ghi duy nhất`"aa"`có thể được tái sử dụng trong toàn bộ công trình. 

Đối với một mô hình xen kẽ hoàn toàn như`"ababab"`, quá trình quét chỉ tạo ra hai chuyển tiếp riêng biệt:`(a,b)`Và`(b,a)`. Mặc dù độ dài chuỗi lớn so với số lần chuyển đổi, cách tiếp cận dựa trên tập hợp sẽ thu gọn chính xác việc lặp lại thành tái sử dụng, xác nhận rằng câu trả lời chỉ phụ thuộc vào các loại kề cận riêng biệt chứ không phải tần số.
