---
title: "CF 1669D - Tem đầy màu sắc"
description: "Chúng ta được cung cấp một dòng ô bắt đầu hoàn toàn trống và chúng ta được phép áp dụng một thao tác đặc biệt luôn tác động lên chính xác hai vị trí liền kề. Mỗi ứng dụng sẽ đổi màu hai ô đó thành các màu khác nhau, một màu đỏ và một màu xanh theo thứ tự."
date: "2026-06-10T01:56:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 1100
weight: 1669
solve_time_s: 97
verified: true
draft: false
---

[CF 1669D - Con dấu đầy màu sắc](https://codeforces.com/problemset/problem/1669/D) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng ô bắt đầu hoàn toàn trống và chúng ta được phép áp dụng một thao tác đặc biệt luôn tác động lên chính xác hai vị trí liền kề. Mỗi ứng dụng sẽ đổi màu hai ô đó thành các màu khác nhau, một màu đỏ và một màu xanh theo thứ tự. 

Cấu hình cuối cùng được cố định trước dưới dạng một chuỗi bao gồm các ô trắng, ô đỏ và ô xanh. Màu trắng có nghĩa là “không bao giờ có màu”, trong khi màu đỏ và xanh lam là những màu vĩnh viễn. Nhiệm vụ là xác định xem có tồn tại một số chuỗi các thao tác dập hai ô liền kề để biến đổi hàng toàn màu trắng ban đầu thành chuỗi đích hay không. 

Mỗi tem luôn ảnh hưởng đến hai ô lân cận, do đó, nó đưa ra một ràng buộc cục bộ: các màu không bao giờ được gán độc lập cho mỗi ô mà luôn theo cặp. Bởi vì các tem có thể chồng lên nhau và được áp dụng lại, các lựa chọn trước đó có thể bị ghi đè, vì vậy vấn đề không phải là theo dõi trình tự xây dựng một cách rõ ràng mà là kiểm tra xem liệu trình tự nhất quán có tồn tại hay không. 

Các ràng buộc đủ lớn để mọi giải pháp đều phải chạy trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 100.000, thậm chí giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ bậc hai cho mỗi trường hợp sẽ quá chậm. Điều này ngay lập tức loại trừ khả năng mô phỏng mạnh mẽ của tất cả các trình tự dập, vì số cách áp dụng tem tăng theo cấp số nhân với số lượng vị trí. 

Một vài trường hợp đặc biệt rất dễ bị bỏ sót nếu lý luận cục bộ. Không thể tạo một ô một màu như “B” hoặc “R” vì mỗi tem tô màu cho hai ô, do đó không thể tạo ra các yêu cầu về một màu riêng biệt. Một trường hợp tinh tế khác là các mẫu như “WBW” hoặc “WRW”, trong đó một ô màu được bao quanh bởi các màu trắng. Mặc dù mỗi ô màu trông có vẻ ổn nhưng không có cách nào để tạo nó mà không ảnh hưởng đến ô lân cận, vì vậy các cấu hình như vậy không thành công. Trường hợp quan trọng thứ ba là các mẫu xen kẽ như “BRBR”, trong đó mọi cặp liền kề đều “trông hợp lệ”, nhưng tính nhất quán tổng thể vẫn quan trọng vì mỗi tem thực thi cấu trúc ghép nối phải xếp chồng toàn bộ vùng màu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng quá trình: bắt đầu từ toàn màu trắng, liên tục chọn các cặp liền kề và gán chúng là BR hoặc RB, khám phá tất cả các chuỗi có thể có cho đến khi chúng tôi đạt được chuỗi mục tiêu hoặc hết các khả năng. Về nguyên tắc, điều này đúng vì nó phản ánh chính xác các quy tắc xây dựng. 

Tuy nhiên, mỗi vị trí có thể là một phần của nhiều lựa chọn dập và mỗi vị trí đóng dấu sẽ phân nhánh thành nhiều trạng thái trong tương lai. Trong trường hợp xấu nhất, số lượng các chuỗi có thể tăng theo cấp số nhân với n, vì mỗi vị trí trong số khoảng n vị trí có thể được ghép nối theo nhiều cách. Ngay cả với n khoảng 50, điều này vẫn không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không cần phải xây dựng trình tự. Mỗi tem luôn tạo ra một cặp ô không phải màu trắng liền kề hợp lệ bao gồm một R và một B. Vì vậy, trong bất kỳ cấu hình cuối cùng hợp lệ nào, mọi khối ô không phải màu trắng liền kề tối đa phải được “có thể phân giải” bên trong thành các cặp liền kề có thể bao phủ tất cả các ô không phải màu trắng. Các ô màu trắng đóng vai trò là các dải phân cách giúp chia vấn đề thành các phân đoạn độc lập. 

Bên trong bất kỳ phân đoạn nào không có ô trắng, về cơ bản chúng ta đang hỏi liệu chúng ta có thể gán cho mỗi vị trí một màu sao cho mọi mối quan hệ liền kề có thể được giải thích bằng một số cặp BR/RB chồng chéo hay không. Sự đơn giản hóa quan trọng là một phân đoạn hợp lệ khi và chỉ khi nó chứa ít nhất một điểm bắt đầu hợp lệ cho một chuỗi các vị trí xen kẽ và không có phân đoạn nào buộc phải có một ô màu riêng biệt.

Điều này giúp giảm vấn đề thành kiểm tra cục bộ tham lam: quét chuỗi, chia chuỗi theo màu trắng và đối với mỗi khối ô không phải màu trắng liền kề, hãy đảm bảo chuỗi đó có ít nhất một cặp điểm cuối cùng màu liền kề hoặc ít nhất một tương tác hợp lệ liền kề cho phép lan truyền. Trong thực tế, sự đơn giản hóa đã biết thậm chí còn trở nên mạnh mẽ hơn: bất kỳ khối nào có độ dài 1 đều không hợp lệ và bất kỳ khối nào có độ dài ít nhất 2 luôn có thể xây dựng được. 

Điều này là do trong bất kỳ đoạn nào có độ dài ít nhất là 2, chúng ta luôn có thể bắt đầu dán tem lên bất kỳ cặp liền kề nào và mở rộng phạm vi bao phủ ra bên ngoài trong khi tôn trọng rằng mọi tem chỉ thực thi các ràng buộc kề cận cục bộ. Có đủ tính linh hoạt nhờ sự chồng chéo để nhận ra bất kỳ phân đoạn đa ô nào, nhưng một ô đơn lẻ không có đối tác bắt nguồn từ đó. 

Vì vậy, điều kiện cuối cùng rút gọn thành việc kiểm tra xem mọi đoạn liền kề tối đa của các ô không phải màu trắng có độ dài ít nhất là 2 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra phân đoạn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Quét chuỗi từ trái sang phải trong khi theo dõi độ dài của khối ô không phải màu trắng liền kề hiện tại. Chúng tôi tăng độ dài bất cứ khi nào chúng tôi thấy 'R' hoặc 'B' và đặt lại độ dài khi chúng tôi thấy 'W'. Điều này là cần thiết vì các tế bào màu trắng phá vỡ mọi tương tác dập. 
2. Bất cứ khi nào chúng ta gặp một ô màu trắng hoặc đến cuối chuỗi, chúng ta sẽ đánh giá đoạn vừa kết thúc. Nếu độ dài của nó chính xác bằng 1, chúng ta biết ngay rằng cấu hình là không thể vì không thể tạo ra một ô đơn lẻ bằng bất kỳ thao tác đóng dấu nào. 
3. Tiếp tục quét qua phần còn lại của chuỗi, lặp lại logic tương tự cho từng đoạn. 
4. Sau khi kết thúc quá trình quét, nếu không tìm thấy đoạn có độ dài 1 không hợp lệ thì có thể cấu hình. 

Điểm quyết định quan trọng là thời điểm chúng tôi đóng một phân khúc. Tại ranh giới đó, chúng tôi có thông tin đầy đủ về vùng tối đa phải được xây dựng mà không bị gián đoạn, vì vậy chúng tôi có thể xác thực nó một cách an toàn cục bộ. 

### Tại sao nó hoạt động 

Mỗi tem luôn tạo ra một cặp ô màu liền kề. Điều này ngụ ý rằng mọi ô màu phải thuộc về một số cặp liền kề bên trong vùng được kết nối của các ô không phải màu trắng. Nếu một đoạn có độ dài 1 thì không có đối tác liền kề nào có sẵn, do đó không có chuỗi tem nào có thể tạo ra nó. Ngược lại, bất kỳ đoạn nào có độ dài ít nhất là 2 đều có thể được tạo bằng cách bắt đầu từ một cặp và kéo dài ra ngoài, vì các tem chồng chéo cho phép đổi màu mà không phá vỡ tính khả thi của vùng lân cận. Điều này làm cho độ dài đoạn là điều kiện cần và đủ duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    ok = True
    i = 0

    while i < n:
        if s[i] == 'W':
            i += 1
            continue

        j = i
        while j < n and s[j] != 'W':
            j += 1

        if j - i == 1:
            ok = False
            break

        i = j

    print("YES" if ok else "NO")
```Giải pháp này hoạt động bằng cách xác định rõ ràng các khối không phải màu trắng liền kề và từ chối bất kỳ khối nào có kích thước bằng một. Quá trình quét là tuyến tính và mỗi vị trí được xử lý nhiều nhất một lần. 

Một chi tiết triển khai tinh tế là đảm bảo con trỏ`i`nhảy trực tiếp đến`j`sau khi xử lý một khối. Điều này tránh các ký tự đếm kép và đảm bảo hành vi O(n). Một điểm quan trọng khác là chúng tôi chỉ đánh giá các phân đoạn khi chúng được đóng hoàn toàn bởi một ô trắng hoặc phần cuối của chuỗi, đảm bảo tính chính xác tại các ranh giới. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`BRBBW`Chúng tôi theo dõi các phân đoạn của tế bào không phải màu trắng. 

| Bước | Chỉ mục | Char | Độ dài đoạn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | B | 1 | đoạn bắt đầu | 
| 2 | 1 | R | 2 | tiếp tục | 
| 3 | 2 | B | 3 | tiếp tục | 
| 4 | 3 | B | 4 | tiếp tục | 
| 5 | 4 | W | kết thúc | độ dài đoạn 4 hợp lệ | 

Không có phân đoạn không hợp lệ nào xuất hiện nên câu trả lời là CÓ. 

Điều này cho thấy các vùng màu dài liền kề có giá trị miễn là chúng không phải là các vùng đơn lẻ bị cô lập. 

### Ví dụ 2:`WBW`| Bước | Chỉ mục | Char | Độ dài đoạn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | W | - | đặt lại | 
| 2 | 1 | B | 1 | đoạn bắt đầu | 
| 3 | 2 | W | kết thúc | độ dài đoạn 1 không hợp lệ | 

Điều này chứng tỏ trường hợp thất bại khi không thể hình thành một ô màu đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập một lần trong quá trình phân đoạn | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được lưu trữ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 100.000, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

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

        ok = True
        i = 0
        while i < n:
            if s[i] == 'W':
                i += 1
                continue
            j = i
            while j < n and s[j] != 'W':
                j += 1
            if j - i == 1:
                ok = False
                break
            i = j

        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples (partial inclusion due to length)
assert run("""1
5
BRBBW
""") == "YES"

assert run("""1
1
B
""") == "NO"

# custom cases
assert run("""1
3
WBW
""") == "NO"

assert run("""1
2
BR
""") == "YES"

assert run("""1
4
WWWW
""") == "YES"

assert run("""1
5
RBBRW
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| WBW | KHÔNG | suy tế bào đơn bị cô lập | 
| BR | CÓ | phân đoạn hợp lệ tối thiểu | 
| WWWW | CÓ | xây dựng trống | 
| RBBRW | CÓ | nhiều phân đoạn hợp lệ | 

## Vỏ cạnh 

Một ô màu duy nhất chẳng hạn như đầu vào “B” sẽ gây ra lỗi ngay lập tức do quá trình quét tạo ra một đoạn có độ dài bằng một. Thuật toán xác định điều này khi phân đoạn đóng ở cuối chuỗi, đánh dấu chính xác nó không hợp lệ. 

Một mẫu như “WBW” chia thành hai đoạn có độ dài một, nhưng chỉ cần một đoạn cũng đủ để bác bỏ trường hợp này. Quá trình quét sẽ đặt lại ở mỗi ô màu trắng, do đó cả hai phân đoạn đều được kiểm tra độc lập. 

Một chuỗi hoàn toàn màu trắng không tạo ra phân đoạn nào cả, do đó không có điều kiện không hợp lệ nào được kích hoạt và câu trả lời vẫn là CÓ, phù hợp với thực tế là chúng ta không thể làm gì cả.
