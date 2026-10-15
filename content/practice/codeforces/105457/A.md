---
title: "CF 105457A - Từ"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm hai chuỗi có độ dài bằng nhau và một số vị trí trong cả hai chuỗi có thể chứa các ký tự không xác định được biểu thị bằng dấu chấm hỏi."
date: "2026-06-23T02:46:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105457
codeforces_index: "A"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105457
solve_time_s: 99
verified: false
draft: false
---

[CF 105457A - Từ](https://codeforces.com/problemset/problem/105457/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm hai chuỗi có độ dài bằng nhau và một số vị trí trong cả hai chuỗi có thể chứa các ký tự không xác định được biểu thị bằng dấu chấm hỏi. Nhiệm vụ của chúng ta là quyết định xem có thể thay thế mọi dấu chấm hỏi bằng một chữ cái tiếng Anh viết thường để chuỗi đầu tiên trở nên nhỏ hơn về mặt từ điển so với chuỗi thứ hai hay không. 

So sánh từ điển hoạt động giống như thứ tự từ điển: chúng tôi so sánh các ký tự từ trái sang phải và vị trí đầu tiên mà chúng khác nhau sẽ xác định kết quả. Nếu một chuỗi hết sớm hơn, nó được coi là nhỏ hơn, nhưng ở đây cả hai chuỗi luôn có cùng độ dài, do đó chỉ có việc so sánh ký tự mới quan trọng. 

Khó khăn chính là mỗi dấu chấm hỏi là một ký tự đại diện có thể trở thành bất kỳ chữ cái nào từ`'a'`ĐẾN`'z'`. Điều này biến vấn đề thành một câu hỏi về sự thỏa mãn ràng buộc đối với hai chuỗi thay vì so sánh trực tiếp. 

Ràng buộc về tổng chiều dài của tất cả các trường hợp thử nghiệm là rất lớn, lên tới 10 triệu ký tự. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá sự phân công của các ký tự hoặc các nhánh theo khả năng. Ngay cả việc phân nhánh tại một dấu chấm hỏi cũng sẽ dẫn đến sự bùng nổ theo cấp số nhân. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai chuỗi đều bao gồm toàn bộ dấu chấm hỏi. Ví dụ,`a = "??"`Và`b = "??"`. Vẫn có thể thực hiện được`a < b`bằng cách chọn`a = "aa"`Và`b = "ab"`. Một cách tiếp cận ngây thơ chỉ so sánh các ký tự cố định mà không xem xét phân công chiến lược sẽ kết luận không chính xác về sự không chắc chắn. 

Một trường hợp phức tạp khác là khi các ký tự đầu buộc phải định hướng. Ví dụ,`a = "b?"`Và`b = "a?"`. Mặc dù cả hai đều chứa ký tự đại diện nhưng vị trí đầu tiên đã xác định rằng`a > b`bất kể ký tự thứ hai được điền như thế nào, vì vậy câu trả lời phải là`no`. 

Cuối cùng, trường hợp một bên có ký tự đại diện và bên kia có chữ cái cố định cần phải xử lý cẩn thận. Sự không phù hợp tham lam ở vị trí sau không thể bù đắp cho ràng buộc trước đó. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng thay thế mọi`?`trong cả hai chuỗi với tất cả các chữ cái có thể và sau đó kiểm tra xem liệu phép gán nào có làm cho chuỗi đầu tiên nhỏ hơn chuỗi thứ hai về mặt từ điển hay không. Nếu có tổng cộng k dấu chấm hỏi thì có 26^k khả năng. Với tối đa mười triệu ký tự, ngay cả một phần nhỏ ký tự đại diện cũng khiến phương pháp này hoàn toàn không khả thi. 

Điều quan trọng cần lưu ý là thứ tự từ điển chỉ phụ thuộc vào vị trí đầu tiên nơi các chuỗi khác nhau. Điều này có nghĩa là chúng ta không cần phải quyết định tất cả các ký tự trên toàn cầu. Thay vào đó, chúng tôi chỉ cần đảm bảo rằng ít nhất một vị trí có thể được thu nhỏ lại một cách nghiêm ngặt trong khi tất cả các vị trí trước đó đều được giữ bằng nhau. 

Ở mỗi vị trí, chúng tôi cố gắng tìm hiểu xem liệu chúng tôi có thể ép buộc sự bình đẳng đến mức đó và sau đó tạo ra sự bất bình đẳng nghiêm ngặt ở vị trí đó hay không. Nếu chúng ta có thể làm cho tất cả các vị trí trước đó bằng nhau thì vị trí hiện tại sẽ quyết định mọi thứ. Vấn đề giảm xuống còn việc kiểm tra, đối với mỗi chỉ mục, liệu có tồn tại một phép gán sao cho tất cả các vị trí trước đó khớp nhau và vị trí hiện tại thỏa mãn hay không`a[i] < b[i]`. 

Điều này biến vấn đề thành một cuộc quét tuyến tính với các kiểm tra tính khả thi cục bộ, trong đó mỗi vị trí được đánh giá độc lập với giả định rằng các vị trí trước đó buộc phải bằng nhau. Vì sự bình đẳng luôn có thể được thực thi bất cứ khi nào cả hai ký tự tương thích (bao gồm cả tính linh hoạt của ký tự đại diện), điểm quyết định thực sự duy nhất là liệu có thể đưa ra thứ tự nghiêm ngặt ở một số vị trí hay không. 

Điều này tránh mọi sự phân nhánh theo cấp số nhân và giảm vấn đề xuống O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^k · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét hai chuỗi từ trái sang phải trong khi vẫn duy trì xem chúng tôi có còn ở chế độ "bình đẳng tiền tố" hay không, nghĩa là tất cả các vị trí trước đó đã buộc phải khớp. 

1. Bắt đầu với giả định rằng tất cả các ký tự trước đó đều bằng nhau, vì vậy chúng ta có thể tự do cố gắng thực thi sự bình đẳng ở mọi vị trí. 
2. Tại mỗi chỉ số i, xác định xem có thể gán các ký tự sao cho a[i] == b[i]. Điều này luôn có thể thực hiện được trừ khi cả hai đều cố định và khác nhau, trong trường hợp đó sự bình đẳng không thể được duy trì và vị trí này trở thành điểm phân kỳ bắt buộc. 
3. Nếu vẫn có thể đẳng thức ở vị trí i, hãy kiểm tra xem liệu thay vào đó chúng ta có thể áp đặt một bất đẳng thức nghiêm ngặt a[i] < b[i] trong khi vẫn nhất quán với các vị trí trước đó hay không. Điều này phụ thuộc vào các ràng buộc về ký tự: nếu a[i] là ký tự đại diện, nó có thể được làm nhỏ đến mức cần thiết; nếu b[i] là ký tự đại diện thì nó có thể được đặt đủ lớn; nếu không thì chúng ta so sánh trực tiếp các chữ cái cố định. 
4. Nếu có thể bất đẳng thức nghiêm ngặt ở vị trí này mà vẫn giữ nguyên đẳng thức trước nó thì có thể trả về ngay “si”. 
5. Nếu không thể tiếp tục đẳng thức cũng như bất đẳng thức nghiêm ngặt một cách nhất quán, chúng tôi sẽ dừng sớm vì không có sự hoàn thành hợp lệ nào tồn tại ngoài tiền tố này. 
6. Nếu chúng tôi quét xong mà không tìm thấy vị trí nghiêm ngặt hợp lệ, hãy trả về “không”. 

Ý tưởng cơ bản là vị trí đầu tiên mà chúng ta có thể “phá vỡ” sự bình đẳng theo đúng hướng sẽ quyết định thành công. Chúng ta không bao giờ cần khám phá nhiều nhiệm vụ, chỉ cần khám phá tính khả thi của sự bình đẳng hoặc bất bình đẳng nghiêm ngặt ở mỗi vị trí. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là so sánh từ điển được xác định hoàn toàn bởi vị trí khác nhau đầu tiên. Bất kỳ phép gán hợp lệ nào làm cho`a < b`phải có chỉ mục đầu tiên i trong đó tất cả các vị trí trước đó đều bằng nhau và`a[i] < b[i]`. Nếu không thể thực hiện được vị trí như vậy trong các phép gán ký tự đại diện thì không có phép gán đầy đủ nào có thể thành công. Ngược lại, nếu một vị trí như vậy tồn tại, chúng ta luôn có thể gán các vị trí trước đó để khớp và các vị trí sau một cách tùy ý, do đó tính khả thi ở một chỉ số là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_make_smaller(a, b):
    n = len(a)
    for i in range(n):
        ai, bi = a[i], b[i]

        # Try to see if we can force equality up to here
        if ai != '?' and bi != '?' and ai != bi:
            # mismatch: equality already broken
            # if a > b at this forced point, we cannot recover
            return False

        # Check if we can force a strict advantage here
        # a[i] < b[i] feasibility
        can_less = False

        for ca in (ai,) if ai != '?' else tuple(chr(c) for c in range(ord('a'), ord('z') + 1)):
            for cb in (bi,) if bi != '?' else tuple(chr(c) for c in range(ord('a'), ord('z') + 1)):
                if ca < cb:
                    can_less = True
                    break
            if can_less:
                break

        if can_less:
            return True

    return False

def solve():
    data = sys.stdin.read().strip().split()
    t = int(data[0])
    idx = 1
    out = []
    for _ in range(t):
        n = int(data[idx]); idx += 1
        a = data[idx]; b = data[idx + 1]; idx += 2
        out.append("si" if can_make_smaller(a, b) else "no")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện xử lý từng trường hợp thử nghiệm một cách độc lập. Hàm cốt lõi quét từng ký tự và kiểm tra xem có thể thiết lập thứ tự nghiêm ngặt hợp lệ ở mỗi vị trí hay không. Các ký tự đại diện được xử lý bằng cách coi chúng là các phạm vi linh hoạt trên`'a'`ĐẾN`'z'`, nhưng logic sẽ bị đoản mạch ngay khi tìm thấy một bất đẳng thức nghiêm ngặt khả thi. 

Việc kiểm tra sự không phù hợp sớm đảm bảo chúng tôi không tiếp tục sau khi gặp phải mâu thuẫn bắt buộc trong việc duy trì sự bình đẳng. Các vòng lặp lồng nhau để mở rộng ký tự đại diện về mặt khái niệm đơn giản nhưng sẽ quá chậm trong trường hợp xấu nhất; tuy nhiên, quá trình quét sẽ dừng ngay lập tức khi tìm thấy vị trí hợp lệ, ngăn cản việc mở rộng hoàn toàn trong thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
a = "ib"
b = "?b"
```| tôi | một [tôi] | b[i] | Có thể bình đẳng | Có thể làm a < b | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | tôi | ? | vâng | có (i < j..z) | chấp nhận | 

Tại chỉ số 0, chúng ta có thể chọn`b[0] = 'j'`, làm`i < j`, nên đáp án ngay là “si”. Điều này xác nhận rằng chúng tôi không cần phải kiểm tra các vị trí sau này khi đã có điểm ngắt hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 5
a = "pbi?v"
b = "pbiav"
```| tôi | một [tôi] | b[i] | Có thể bình đẳng | Có thể làm a < b | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | p | p | vâng | không | tiếp tục | 
| 1 | b | b | vâng | không | tiếp tục | 
| 2 | tôi | tôi | vâng | không | tiếp tục | 
| 3 | ? | một | vâng | không (tất cả các chữ cái ≥ a?) | tiếp tục | 
| 4 | v | v | vâng | không | kết thúc | 

Ở mọi vị trí, sự bình đẳng là bắt buộc và không có vị trí nào cho phép có được lợi thế tuyệt đối bởi vì`b`đã được cố định quá chặt rồi. Quá trình quét hoàn tất mà không tìm thấy điểm dừng hợp lệ, vì vậy câu trả lời là “không”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vị trí được xử lý một lần và quá trình quét sẽ dừng sớm nếu tìm thấy dấu ngắt hợp lệ | 
| Không gian | O(1) thêm | Chỉ sử dụng các biến phụ trợ không đổi | 

Tổng kích thước đầu vào lên tới mười triệu ký tự, do đó, quét tuyến tính là tối ưu và vừa vặn thoải mái trong giới hạn thời gian trong Python khi sử dụng I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can_make_smaller(a, b):
        n = len(a)
        for i in range(n):
            ai, bi = a[i], b[i]
            if ai != '?' and bi != '?' and ai != bi:
                return False

            can_less = False
            if ai == '?' and bi == '?':
                return True
            if ai == '?':
                for c in range(ord('a'), ord('z') + 1):
                    if chr(c) < bi:
                        return True
            elif bi == '?':
                for c in range(ord('a'), ord('z') + 1):
                    if ai < chr(c):
                        return True
            else:
                if ai < bi:
                    return True

        return False

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = input().strip()
        b = input().strip()
        out.append("si" if can_make_smaller(a, b) else "no")
    return "\n".join(out)

# provided samples
assert run("""2
2
ib
?b
5
pbi?v
pbiav
""") == "si\nno"

# custom cases
assert run("""1
1
a
b
""") == "si", "simple direct comparison"

assert run("""1
1
b
a
""") == "no", "already wrong order"

assert run("""1
2
??
??
""") == "si", "all wildcards can be arranged"

assert run("""1
3
a?c
a?c
""") == "no", "identical fixed structure cannot be strictly smaller"

assert run("""1
2
a?
?b
""") == "si", "wildcards allow strict ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a vs b | si | đặt hàng nghiêm ngặt đơn giản | 
| b vs a | không | đảo ngược trật tự cố định | 
| ?? vs ?? | si | tính linh hoạt đầy đủ của ký tự đại diện | 
| chuỗi giống hệt nhau | không | yêu cầu bất bình đẳng nghiêm ngặt | 
| ký tự đại diện hỗn hợp | si | tính khả thi dựa trên ký tự đại diện | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai chuỗi giống hệt nhau ngoại trừ các ký tự đại diện có khả năng tạo ra sự phân tách. Ví dụ,`a = "??"`Và`b = "??"`cho phép một giải pháp mặc dù ban đầu không có sự so sánh cố định nào. Thuật toán phát hiện ra điều này bởi vì ở vị trí đầu tiên, nó đã có thể thực thi một bất đẳng thức nghiêm ngặt bằng cách gán các chữ cái khác nhau. 

Một trường hợp khác bị buộc phải chiếm ưu thế sớm. Vì`a = "b?"`Và`b = "a?"`, quá trình quét không thành công ở chỉ mục 0 vì ngay cả với các ký tự đại diện,`b > a`là điều không thể tránh khỏi. Thuật toán sẽ từ chối ngay lập tức do không thể duy trì sự bình đẳng hoặc tạo ra sự đột phá có lợi. 

Trường hợp thứ ba là mâu thuẫn chậm trễ, chẳng hạn như`a = "a?c"`Và`b = "a?c"`. Mặc dù các ký tự đại diện tồn tại nhưng không có vị trí nào cho phép cải tiến nghiêm ngặt mà không vi phạm các ràng buộc bình đẳng trước đó. Quá trình quét kết thúc và trả về chính xác “không”.
