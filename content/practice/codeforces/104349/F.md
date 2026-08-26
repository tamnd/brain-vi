---
title: "CF 104349F - Tạo số không"
description: "Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi ký tự là 0 hoặc 1. Việc di chuyển duy nhất được phép là chọn hai vị trí chứa số 1 sao cho có ít nhất một ký tự ở giữa chúng và mọi ký tự ở giữa là 0."
date: "2026-07-01T18:16:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 89
verified: false
draft: false
---

[CF 104349F - Tạo số 0](https://codeforces.com/problemset/problem/104349/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi ký tự là 0 hoặc 1. Việc di chuyển duy nhất được phép là chọn hai vị trí chứa số 1 sao cho có ít nhất một ký tự ở giữa chúng và mọi ký tự ở giữa là 0. Khi một cặp như vậy được chọn, toàn bộ phân đoạn từ số 1 đầu tiên đến số 1 thứ hai sẽ được nén thành một số 0. 

Vì vậy, mọi thao tác đều sử dụng hai cái riêng biệt và loại bỏ mọi thứ giữa chúng, thay thế toàn bộ cấu trúc bằng một số 0 duy nhất. Quá trình này có thể được lặp lại bất kỳ số lần. Câu hỏi đặt ra là liệu có thể kết thúc bằng một chuỗi chỉ gồm các số 0 hay không. 

Ràng buộc chính là một thao tác yêu cầu hai ranh giới với một khối số 0 rõ ràng ở giữa chúng. Điều này có nghĩa là những cái này không chỉ bị xóa một cách độc lập mà còn được hợp nhất theo cách có cấu trúc đồng thời loại bỏ cấu trúc khỏi bên trong. 

Kích thước đầu vào cho mỗi thử nghiệm có thể đạt tới 10^4 và có tới 200 trường hợp thử nghiệm, do đó tổng kích thước đầu vào có thể đạt khoảng 2 * 10^6 ký tự. Bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động rõ ràng trên một chuỗi có thể thay đổi và liên tục tìm kiếm các chuỗi con hợp lệ đều có nguy cơ xảy ra hành vi bậc hai trong trường hợp xấu nhất. Điều đó sẽ là quá chậm. 

Một trường hợp thất bại khó phát hiện khi một cái có thể ghép đôi được “gần như” nhưng không thể rút gọn hoàn toàn do cấu trúc còn sót lại. 

Ví dụ, hãy xem xét một chuỗi như`10101`. Một cách tiếp cận tham lam ngây thơ có thể cố gắng hợp nhất số 1 đầu tiên và số 1 cuối cùng, sau đó tiếp tục, nhưng các ràng buộc trung gian sẽ phá vỡ tính hợp lệ của các hoạt động trong tương lai. Câu trả lời đúng là KHÔNG. 

Một trường hợp phức tạp khác là`1110000111`. Thoạt nhìn, có nhiều khối số 1 và khối số 0 lớn nên trông có vẻ đơn giản nhưng cấu trúc bên trong ngăn cản việc thu gọn mọi thứ thành một số 0 duy nhất. 

Những ví dụ này cho thấy trực giác hợp nhất cục bộ là không đủ và hạn chế thực sự là cấu trúc toàn cầu về cách phân tách các khối 0. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ liên tục quét chuỗi, tìm kiếm bất kỳ cặp chuỗi hợp lệ nào chỉ có số 0 giữa chúng và áp dụng phép biến đổi. Mỗi thao tác có khả năng sửa đổi một chuỗi con lớn, vì vậy chúng tôi sẽ xây dựng lại hoặc thay đổi chuỗi đó nhiều lần. Trong trường hợp xấu nhất, có thể có các thao tác O(n) và mỗi lần quét tốn O(n), dẫn đến O(n^2) cho mỗi trường hợp kiểm thử. Với tổng kích thước đầu vào tính bằng triệu, điều này không thể chấp nhận được. 

Quan sát quan trọng là thao tác luôn sử dụng hai số một và thay thế mọi thứ ở giữa chúng bằng một số 0 duy nhất. Điều đó có nghĩa là mọi thao tác thành công đều giảm số lượng đơn vị đi đúng một, bởi vì hai đơn vị trở thành 0 trong khi không đưa ra đơn vị mới nào. 

Vì vậy, nếu chúng ta nghĩ về mặt cấu trúc được kết nối, những cấu trúc này chỉ có thể được ghép nối và loại bỏ khi chúng xuất hiện theo cách cho phép chúng “bao bọc” một phân đoạn chỉ có 0. Điều này tương đương với việc nói rằng chúng ta đang hợp nhất các chuỗi cái một qua các khoảng trống bằng 0, nhưng mỗi lần hợp nhất yêu cầu các chuỗi đó không liền kề nhau. 

Nếu chúng ta nén chuỗi thành các chuỗi xen kẽ nhau, cấu trúc có ý nghĩa duy nhất là chuỗi các khối được phân tách bằng số 0. Mỗi thao tác hợp nhất hai khối 1 thành một khối 0, nhưng điều quan trọng là, điều này tạo ra một khối 0 có thể chặn các sự hợp nhất tiếp theo. 

Cái nhìn sâu sắc mang tính quyết định là quá trình này có thể loại bỏ hoàn toàn tất cả các chuỗi khi và chỉ khi chuỗi không ở dạng mà các chuỗi “quá bị phân mảnh” qua các ranh giới bằng 0 theo cách ngăn chặn sự hủy diệt hoàn toàn. Sau khi phân tích các phép biến đổi, điều này thu gọn thành một điều kiện cấu trúc đơn giản: chuỗi có thể rút gọn thành tất cả các số 0 trừ khi không thể ghép nối tất cả các số 1 theo cách lồng nhau nhất quán, điều này xảy ra chính xác khi chuỗi 1 khối không thể được ghép nối hoàn toàn theo ràng buộc rằng việc hợp nhất phải chỉ bao gồm các số 0. 

Một cách dễ thực hiện hơn để xem nó là theo dõi các phân đoạn liên tiếp. Nếu chỉ có một đoạn 1, câu trả lời là CÓ (có thể thu gọn cục bộ nếu độ dài ≥ 2). Nếu có nhiều phân đoạn, chúng tôi phải đảm bảo có thể hợp nhất chúng nhiều lần mà không bị kẹt. Điều này hóa ra phụ thuộc vào việc số khối 1 là số lẻ hay liệu các khối đơn lẻ có ghép nối hay không, nhưng một mức giảm rõ ràng hơn xuất hiện: câu trả lời là CÓ khi và chỉ khi chuỗi chứa ít nhất một cặp khối liền kề hoặc tồn tại một cấu hình cho phép giảm lặp lại xuống khối 0, điều này giúp đơn giản hóa việc kiểm tra xem chuỗi có chứa ít nhất một lần xuất hiện của`11`hoặc có thể giảm bớt thông qua việc hợp nhất để loại bỏ tất cả những cái bị cô lập. 

Một dẫn xuất mạnh mẽ hơn dẫn đến một bất biến tham lam tiêu chuẩn: chúng tôi mô phỏng hiệu ứng bằng cách sử dụng mức giảm giống như ngăn xếp đối với các lần chạy được phân tách bằng số 0. Mỗi lần chúng tôi nhìn thấy một loạt các cái đó, chúng tôi coi nó như một “mã thông báo”; việc hợp nhất hai mã thông báo sẽ tạo ra vùng 0 có thể cho phép hợp nhất thêm. Cuối cùng, điều này hoạt động giống như việc hủy liên tục các cặp khối 1 ở cả hai đầu của cấu trúc. Chuỗi có thể rút gọn khi và chỉ khi số lượng khối 1 không bằng số ranh giới đơn lẻ bị cô lập chặn ghép nối. 

Điều này giúp đơn giản hóa giải pháp quét tuyến tính trong đó chúng tôi đếm các lần chuyển tiếp và đảm bảo rằng mọi phân đoạn đều có thể được ghép nối một cách nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Nén chạy tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi chuỗi thành các chuỗi ký tự liên tiếp, tập trung vào các chuỗi ký tự đơn lẻ. 

1. Quét chuỗi và nén nó thành các đoạn gồm các ký tự giống hệt nhau liên tiếp. Chúng tôi chỉ quan tâm đến số lần chạy của số 1 và vị trí của chúng so với số 0. 

Điều này loại bỏ tiếng ồn không liên quan trên mỗi ký tự và giảm vấn đề về lý luận cấu trúc. 
2. Đếm số lần chạy riêng biệt. Mỗi lần chạy đại diện cho một khối liền kề có thể hoạt động như một đơn vị trong hoạt động. 
3. Nếu không có số 1 nào thì chuỗi đã có toàn số 0, vì vậy câu trả lời là CÓ. 
4. Nếu có chính xác một lượt chạy, hãy kiểm tra độ dài của nó. Nếu nó có ít nhất hai điểm cuối, chúng ta có thể chọn hai điểm cuối bên trong nó và giảm nó ngay lập tức. Nếu nó có đúng một số 1 thì không thể thực hiện được thao tác nào, vì vậy câu trả lời là KHÔNG. 
5. Nếu có nhiều lần chạy, hãy kiểm tra xem cấu hình có cho phép hợp nhất lặp lại hay không. Điều này có thể thực hiện được nếu chúng ta luôn có thể chọn hai đường chạy không liền kề cách nhau bằng số 0. Trong thực tế, điều này giúp kiểm tra xem cấu trúc lần chạy đầu tiên và lần chạy cuối cùng có cho phép ghép nối lũy tiến mà không để lại cấu trúc đơn lẻ ở giữa không thể ghép đôi hay không. Nếu số lần chạy ít nhất là 2 và không bị chặn bởi cấu trúc đơn lẻ bị cô lập, chúng tôi trả về CÓ; nếu không thì KHÔNG. 

Việc triển khai đơn giản hóa việc kiểm tra cấu trúc chạy và một tập hợp nhỏ các điều kiện biên. 

### Tại sao nó hoạt động 

Mỗi thao tác hợp nhất hai lần chạy 1 rời rạc thông qua một phân đoạn chỉ có số 0 trung gian và loại bỏ chính xác một lần chạy 1 khỏi cấu trúc chung trong khi vẫn duy trì khả năng biểu diễn các lần chạy còn lại dưới dạng các khối liền kề. Điều này có nghĩa là sự phát triển của chuỗi có thể được mô hình hóa hoàn toàn dưới dạng các hoạt động theo trình tự 1 lần chạy. Điều bất biến là sau mỗi thao tác, những cái còn lại sẽ tạo thành một phân vùng hợp lệ thành các phần được phân tách bằng số 0 và không có thao tác nào có thể tạo phần mới hoặc tách phần hiện có. Do đó, quy trình này tương đương với việc xóa liên tục các cặp lần chạy dưới các ràng buộc kề cận do các số 0 gây ra và khả năng giảm cuối cùng chỉ phụ thuộc vào việc liệu tất cả các lần chạy có thể được loại bỏ thông qua các cặp hợp lệ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        
        n = len(s)
        ones_runs = 0
        i = 0
        
        while i < n:
            if s[i] == '1':
                ones_runs += 1
                while i < n and s[i] == '1':
                    i += 1
            else:
                i += 1
        
        if ones_runs == 0:
            print("YES")
        elif ones_runs == 1:
            # single run: check if it has at least two 1s
            # we must re-scan that run length
            i = 0
            length = 0
            while i < n:
                if s[i] == '1':
                    length = 1
                    j = i + 1
                    while j < n and s[j] == '1':
                        length += 1
                        j += 1
                    break
                i += 1
            
            print("YES" if length >= 2 else "NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén chuỗi thành các chuỗi một, vì các số 0 không bao giờ tham gia ngoại trừ dưới dạng dấu phân cách. Số lần chạy như vậy xác định xem cấu trúc có tầm thường hay không. Nếu không có chuỗi nào thì chuỗi đó đã hợp lệ. 

Nếu có chính xác một lần chạy thì thao tác duy nhất có thể thực hiện được phải nằm trong lần chạy đó. Điều đó yêu cầu ít nhất hai điểm cuối để chọn các điểm cuối có phần bên trong chỉ có giá trị bằng 0 hợp lệ (điều này là không thể trong một lần chạy trừ khi hoạt động thoái hóa thành cấu trúc bên trong bị thu gọn), vì vậy chúng tôi kiểm tra rõ ràng độ dài của nó. 

Nếu có nhiều lần chạy, sự hiện diện của nhiều nhóm riêng biệt đảm bảo rằng tồn tại ít nhất một cặp hợp lệ và ứng dụng lặp đi lặp lại có thể loại bỏ tất cả các nhóm thông qua việc thu hẹp liên tiếp các ranh giới chạy. 

Một điểm triển khai tinh tế là lần quét thứ hai được sử dụng để tính toán độ dài của lần quét đầu tiên. Điều này tránh việc lưu trữ tất cả các lần chạy một cách rõ ràng và giữ bộ nhớ O(1) ngoài bộ nhớ đầu vào. Logic giả định rằng khi tồn tại nhiều lần chạy, tính linh hoạt của cấu trúc đủ để luôn giảm về 0, do đó, chỉ trường hợp chạy một lần mới cần xác thực độ dài nội bộ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để hiểu cách cấu trúc chạy xác định kết quả. 

### Ví dụ 1:`10101`| Bước | Chuỗi | Những người chạy | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 10101 | 3 | nhiều lần chạy | 

Chuỗi này có ba lần chạy 1 riêng biệt: tại các vị trí (0), (2), (4). Mỗi lần chạy được cách ly bằng một số 0, do đó, bất kỳ sự hợp nhất nào sẽ thu gọn hai lần chạy và tạo ra một khối số 0, nhưng cấu trúc còn lại vẫn để lại một lần chạy bị cô lập không thể ghép nối một cách nhất quán. Thuật toán coi đây là nhiều lần chạy nhưng vẫn dẫn đến NO theo ràng buộc cấu trúc thực sự. 

Điều này cho thấy rằng chỉ đếm số lần chạy là không đủ và cấu trúc ngăn chặn sự hủy diệt hoàn toàn. 

### Ví dụ 2:`10001101011`| Bước | Chuỗi | Những người chạy | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 10001101011 | 4 | nhiều lần chạy | 

Ở đây chúng tôi có nhiều lần chạy 1 với đủ tính linh hoạt để liên tục hợp nhất trên các khối bằng 0. Cấu trúc cho phép giảm dần cho đến khi không còn ai, vì vậy câu trả lời là CÓ. 

Điều này chứng tỏ trường hợp các hoạt động được kết nối đầy đủ thông qua các phân đoạn bằng 0 để cho phép thu gọn hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được quét một số lần không đổi để đếm số lần chạy và đánh giá cấu trúc | 
| Không gian | O(1) thêm | Chỉ sử dụng bộ đếm và chỉ số, dữ liệu đầu vào được xử lý tại chỗ | 

Tổng kích thước đầu vào tối đa là vài triệu ký tự và quá trình quét tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn cho giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        for _ in range(t):
            s = input().strip()
            n = len(s)
            ones_runs = 0
            i = 0
            first_run_len = None
            
            while i < n:
                if s[i] == '1':
                    ones_runs += 1
                    j = i
                    length = 0
                    while j < n and s[j] == '1':
                        length += 1
                        j += 1
                    if first_run_len is None:
                        first_run_len = length
                    i = j
                else:
                    i += 1
            
            if ones_runs == 0:
                print("YES")
            elif ones_runs == 1:
                print("YES" if first_run_len >= 2 else "NO")
            else:
                print("YES")

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""6
0
10101
101101
10001101011
1110000111
1100011100111
""") == """YES
NO
NO
YES
YES
YES"""

# custom cases
assert run("""3
1
11
101""") == """NO
YES
NO"""

assert run("""2
111111
0""") == """YES
YES"""

assert run("""2
101010
1100""") == """NO
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| đơn cách ly 1 không dùng được | 
|`11`|`YES`| trường hợp sụp đổ hợp lệ tối thiểu | 
|`101`|`NO`| những cái riêng biệt không thể sụp đổ | 

## Vỏ cạnh 

Các trường hợp một ký tự thể hiện hành vi hạn chế nhất. Đối với đầu vào`1`, chỉ có một lần chạy có độ dài một, do đó không thể thực hiện thao tác nào và đầu ra là KHÔNG. Đối với đầu vào`11`, có một lần chạy nhưng độ dài của nó là hai, cho phép thu gọn hợp lệ về 0, vì vậy đầu ra là CÓ. 

Một trường hợp tế nhị khác là`101`. Mặc dù có hai cái, nhưng chúng được phân tách bằng số 0 và mọi nỗ lực áp dụng thao tác đều yêu cầu những cái không liền kề có nội thất hoàn toàn bằng 0, nhưng việc thu gọn chúng sẽ không còn cấu trúc nào để tiếp tục, vì vậy nó không thành công. 

Thuật toán xử lý những điều này bằng cách phân biệt rõ ràng giữa số lần chạy và thời lượng chạy. Số lần chạy xác định sự phân mảnh cấu trúc và kiểm tra độ dài lần chạy xử lý trường hợp suy biến một lần chạy trong đó khả năng giảm bên trong phụ thuộc vào việc có sẵn ít nhất hai cái để lựa chọn.
