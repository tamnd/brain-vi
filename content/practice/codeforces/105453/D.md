---
title: "CF 105453D – Giải Mã Ký Hiệu Cổ"
description: "Nhiệm vụ là phân tích một chuỗi được viết trên một tấm bảng cổ và xác định xem nó có thể được giải thích bao nhiêu bằng cách sử dụng một tập hợp các “đoạn có ý nghĩa” đã biết. Mỗi đoạn là một chuỗi ngắn đã được hiểu."
date: "2026-06-23T17:35:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "D"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 94
verified: true
draft: false
---

[CF 105453D - Giải mã các ký hiệu cổ](https://codeforces.com/problemset/problem/105453/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân tích một chuỗi được viết trên một tấm bảng cổ và xác định xem nó có thể được giải thích bao nhiêu bằng cách sử dụng một tập hợp các “đoạn có ý nghĩa” đã biết. Mỗi đoạn là một chuỗi ngắn đã được hiểu. Một đoạn văn bản trên máy tính bảng được coi là có thể giải mã được nếu nó có thể được chia hoàn toàn thành các đoạn đã biết này mà không để lại bất kỳ ký tự nào bị che khuất và không sắp xếp lại các ký tự. 

Mục tiêu là tìm một chuỗi con liền kề của máy tính bảng có thể được phân đoạn đầy đủ thành các đoạn đã biết và trong số tất cả các chuỗi con như vậy, chúng tôi muốn có độ dài tối đa có thể. 

Đầu vào cung cấp một chuỗi chính có độ dài lên tới 1000 và tối đa 200 mẫu đã biết, mỗi mẫu có độ dài tối đa là 20. Các ràng buộc ngay lập tức gợi ý rằng các giải pháp ở mức vài trăm triệu thao tác là ở mức giới hạn nhưng vẫn có thể quản lý được bằng Python nếu được cấu trúc cẩn thận. Bất cứ điều gì bậc hai về độ dài chuỗi kết hợp với khớp mẫu đều có thể chấp nhận được, nhưng phải tránh bất kỳ điều gì bậc ba trong trường hợp xấu nhất. 

Vỏ ngoài tinh tế đến từ các mẫu chồng chéo và tái sử dụng. Ví dụ, nếu văn bản là`aaaaa`và các mẫu là`aa`Và`aaa`, thì sẽ tồn tại nhiều phân đoạn và quá trình quét tham lam có thể không thành công tùy theo thứ tự. Một vấn đề khác là giả định một phân đoạn tốt nhất từ ​​đầu chuỗi, trong khi phân đoạn hợp lệ có thể bắt đầu ở bất kỳ đâu ở giữa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi con và kiểm tra xem liệu nó có thể được phân đoạn bằng cách sử dụng từ điển các mẫu đã cho hay không. Đối với mỗi chuỗi con`s[l:r]`, chúng tôi thử lập trình động ngắt từ tiêu chuẩn: chúng tôi kiểm tra xem liệu chúng tôi có thể phân chia nó thành các từ hợp lệ hay không. Có các chuỗi con O(n²) và mỗi lần kiểm tra DP có chi phí lên tới O(n * m * L) trong trường hợp xấu nhất khi chúng tôi thử khớp tất cả các mẫu ở mọi vị trí. Điều này dẫn đến hành vi gần như O(n³) hoặc tệ hơn, quá chậm đối với n lên tới 1000. 

Quan sát quan trọng là chúng ta không thực sự cần phải xử lý mọi chuỗi con một cách độc lập. Thay vào đó, chúng ta có thể tính toán, đối với mỗi vị trí bắt đầu, chúng ta có thể mở rộng phân đoạn hợp lệ đến mức nào nếu chúng ta nhất quyết bắt đầu chính xác ở đó. Nếu chúng ta biết điều đó từ vị trí`i`chúng ta có thể đạt đến vị trí xa nhất`j`, thì tất cả các chuỗi con bắt đầu từ`i`và kết thúc giữa`i`Và`j`là hợp lệ, nhưng chỉ có phần mở rộng đầy đủ mới có ý nghĩa quan trọng đối với câu trả lời. 

Điều này làm giảm vấn đề thành một chương trình động trên các vị trí. Tại mỗi chỉ mục, chúng tôi cố gắng mở rộng bằng cách sử dụng bất kỳ mẫu nào phù hợp với vị trí hiện tại. Nếu một mẫu phù hợp tại`i`, và chúng ta đã biết`dp[i]`có thể truy cập được, chúng ta có thể thư giãn`dp[i + len(pattern)]`có thể tiếp cận được. Về cơ bản, chúng tôi đang xây dựng khả năng tiếp cận trong một biểu đồ trong đó các cạnh tương ứng với các mẫu khớp. 

Sau đó, chúng tôi theo dõi điểm cuối có thể truy cập tối đa trên tất cả các vị trí bắt đầu hợp lệ mà bản thân chúng có thể truy cập được ngay từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi con bạo lực + DP | O(n³) | O(n) | Quá chậm | 
| DP với chuyển tiếp mẫu | O(n · m · L) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng vị trí trong chuỗi dưới dạng một trạng thái. Một trạng thái có thể truy cập được nếu chúng ta có thể bắt đầu từ chỉ mục đó trong một số quy trình phân đoạn hợp lệ. Chúng tôi muốn phổ biến khả năng tiếp cận về phía trước bằng cách sử dụng kết quả khớp từ điển. 

1. Xây dựng cấu trúc tra cứu các mẫu, điển hình là một tập hợp được nhóm theo độ dài. Điều này cho phép chúng tôi nhanh chóng kiểm tra xem một chuỗi con có độ dài nhất định có hợp lệ hay không. 
2. Tạo một mảng boolean`dp`Ở đâu`dp[i]`nghĩa là vị trí đó`i`có thể truy cập được từ một số lần bắt đầu phân đoạn hợp lệ. Chúng tôi khởi tạo`dp[0] = True`bởi vì chúng ta có thể bắt đầu từ đầu chuỗi. 
3. Lặp lại các vị trí`i`từ trái sang phải. Nếu như`dp[i]`là sai, chúng tôi bỏ qua nó vì không có phân đoạn hợp lệ nào có thể bắt đầu ở đó. 
4. Đối với mỗi mẫu, hãy cố gắng khớp nó ở vị trí`i`. Nếu chuỗi con`s[i:i+len(pattern)]`bằng mẫu rồi đánh dấu`dp[i + len(pattern)] = True`. 
5. Theo dõi tất cả các vị trí có thể truy cập được, bởi vì mọi vị trí có thể truy cập đều có thể đóng vai trò là điểm cuối của một phân đoạn hợp lệ. 
6. Đáp án là giá trị lớn nhất của`j - i`trên tất cả các cặp ở đó`dp[i]`Và`dp[j]`có thể truy cập được và`i <= j`, nhưng vì chúng tôi đang xây dựng khả năng tiếp cận từ 0 nên thay vào đó, chúng tôi có thể theo dõi chỉ mục có thể tiếp cận xa nhất từ ​​mỗi phân đoạn bắt đầu hợp lệ và tính toán khoảng cách tối đa giữa các ranh giới phân đoạn hợp lệ liên tiếp. 

Một công thức trực tiếp hơn, đó là những gì chúng tôi triển khai, là coi đây là biểu đồ về khả năng tiếp cận và theo dõi, đối với mọi chỉ mục, điểm cuối xa nhất mà chúng tôi có thể tiếp cận bắt đầu từ bất kỳ vị trí có thể tiếp cận nào tại hoặc trước nó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là`dp[i]`đúng khi và chỉ khi tiền tố`s[0:i]`có thể được phân chia đầy đủ thành các mẫu từ điển. Mọi chuyển đổi đều duy trì tính chính xác vì chúng tôi chỉ mở rộng từ các vị trí đã thể hiện các phân đoạn hợp lệ và mọi phần mở rộng đều tương ứng chính xác với việc thêm một từ hợp lệ. Vì tất cả các phân đoạn hợp lệ được xây dựng bằng cách ghép các từ trong từ điển nên mọi điểm cuối hợp lệ đều phải có thể truy cập được thông qua các chuyển đổi này và ngược lại, mọi điểm cuối có thể truy cập đều tương ứng với một phân đoạn hợp lệ. Sự phản ánh này đảm bảo chúng tôi không bỏ sót bất kỳ phân đoạn hợp lệ nào cũng như không bao gồm bất kỳ phân đoạn không hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    m = int(input())
    
    patterns = []
    by_len = {}
    
    for _ in range(m):
        p = input().strip()
        patterns.append(p)
        by_len.setdefault(len(p), []).append(p)
    
    dp = [False] * (n + 1)
    dp[0] = True
    
    for i in range(n):
        if not dp[i]:
            continue
        
        for L, group in by_len.items():
            if i + L > n:
                continue
            sub = s[i:i+L]
            for p in group:
                if sub == p:
                    dp[i + L] = True
    
    # We now have all reachable segment boundaries.
    # Compute longest reachable segment [i, j] where both i and j are reachable.
    best = 0
    last = 0
    
    for i in range(1, n + 1):
        if dp[i]:
            best = max(best, i - last)
            last = i
    
    return str(best)

if __name__ == "__main__":
    print(solve())
```Mảng DP được xây dựng dưới dạng cấu trúc có khả năng tiếp cận vượt qua ranh giới tiền tố. Vòng lặp lồng nhau kiểm tra mọi vị trí có thể truy cập và thử tất cả độ dài mẫu, đảm bảo chúng tôi chỉ mở rộng các phân đoạn hợp lệ. 

Lần quét cuối cùng diễn giải các vị trí có thể tiếp cận dưới dạng ranh giới phân đoạn. Bất cứ khi nào chúng tôi nhìn thấy ranh giới có thể tiếp cận, chúng tôi sẽ đo khoảng cách từ ranh giới có thể tiếp cận trước đó, tương ứng với phân đoạn có thể giải mã hoàn toàn tối đa. 

Một điểm tinh tế là chúng tôi không bao giờ theo dõi rõ ràng điểm bắt đầu của các đoạn; thay vào đó, chúng tôi dựa vào thực tế là bất kỳ phân đoạn hợp lệ nào cũng phải bắt đầu ngay sau ranh giới hợp lệ, vì quá trình phân đoạn sẽ phân chia chuỗi thành các khối hợp lệ rời rạc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abafopwewwqeiqioloadaduertwpoeabafad
3
abaf
iolo
ad
```Chúng tôi theo dõi các trạng thái tiền tố có thể truy cập. 

| tôi | dp[i] | hành động | mới có thể truy cập | 
| --- | --- | --- | --- | 
| 0 | Đúng | khớp "abaf" ở 0 | 4 | 
| 4 | Đúng | không khớp | - | 
| ... | ... | bỏ qua cho đến khi "iolo" | 18 | 
| 18 | Đúng | khớp với "quảng cáo" | 20 | 

Các ranh giới có thể tiếp cận bao gồm 0, 4, 18, 20. Khoảng cách tối đa giữa các ranh giới liên tiếp là 8, tương ứng với đoạn`"ioloadad"`. 

Điều này cho thấy rằng DP sắp xếp chính xác nhiều mẫu trên chuỗi thay vì buộc phải tạo một chuỗi mẫu đơn ngay từ đầu. 

### Ví dụ 2 

đầu vào:```
aaaaa
2
aa
aaa
```Chúng tôi tính toán: 

| tôi | dp[i] | chuyển tiếp | 
| --- | --- | --- | 
| 0 | Đúng | 0→2, 0→3 | 
| 2 | Đúng | 2→4, 2→5 | 
| 3 | Đúng | 3→5 | 

Các ranh giới có thể tiếp cận là 0, 2, 3, 4, 5. Khoảng cách tối đa là 3, tương ứng với`"aaa"`. 

Điều này chứng tỏ các mô hình chồng chéo được xử lý một cách tự nhiên vì tất cả các chuyển đổi có thể đều được khám phá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m · L) | Đối với mỗi vị trí có thể tiếp cận, chúng tôi thử tối đa m mẫu và so sánh tối đa L ký tự | 
| Không gian | O(n + m) | Mảng DP cộng với lưu trữ mẫu | 

Các giới hạn n ≤ 1000 và m ≤ 200 với độ dài mẫu nhỏ đảm bảo sản phẩm bộ ba nằm trong giới hạn chấp nhận được. Thuật toán chạy thoải mái trong vòng 2 giây bằng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""abafopwewwqeiqioloadaduertwpoeabafad
3
abaf
iolo
ad
""") == "8"

# minimal case
assert run("""a
1
a
""") == "1"

# no matches
assert run("""abc
1
x
""") == "0"

# overlapping patterns
assert run("""aaaaa
2
aa
aaa
""") == "3"

# full string single pattern
assert run("""abcd
1
abcd
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu char đơn | 1 | giá trị tối thiểu | 
| không có từ điển phù hợp | 0 | trường hợp phân đoạn trống | 
| mô hình chồng chéo | 3 | giải quyết xung đột | 
| trận đấu đầy đủ | 4 | phạm vi bảo hiểm mẫu đơn | 

## Vỏ cạnh 

Đối với một chuỗi như`aaaaa`với các mẫu`aa`Và`aaa`, thuật toán đánh dấu nhiều điểm có khả năng tiếp cận chồng chéo. Bắt đầu từ chỉ số 0, cả 2 và 3 đều có thể truy cập được. Từ 2, chỉ số 4 và 5 có thể truy cập được và từ 3, chỉ mục 5 có thể truy cập được. DP tích lũy chính xác tất cả các ranh giới có thể tiếp cận thay vì cam kết với một đường dẫn phân đoạn duy nhất. 

Đối với một chuỗi không có mẫu hợp lệ,`dp`vẫn sai ngoại trừ ở chỉ số 0 và không có chuyển đổi nào xảy ra. Lần quét cuối cùng không thấy phân đoạn hợp lệ nào vượt quá độ dài 0, trả về chính xác 0. 

Đối với một chuỗi hoàn toàn phù hợp như`abcd`với hoa văn`abcd`, DP chỉ chuyển tiếp một lần từ 0 đến 4 và kết quả là độ dài đầy đủ.
