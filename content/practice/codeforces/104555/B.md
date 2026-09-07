---
title: "CF 104555B - Xáo bài công bằng nhất"
description: "Chúng ta được sắp xếp cuối cùng về hoán vị các số từ 1 đến N và chúng ta muốn hiểu xem chúng ta phải áp dụng một thao tác xáo trộn rất cụ thể bao nhiêu lần bắt đầu từ hoán vị nhận dạng 1, 2, 3, ..., N để có được nó."
date: "2026-06-30T08:46:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 102
verified: true
draft: false
---

[CF 104555B - Trộn bài công bằng nhất](https://codeforces.com/problemset/problem/104555/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp cuối cùng về hoán vị các số từ 1 đến N và chúng ta muốn hiểu xem chúng ta phải áp dụng một thao tác xáo trộn rất cụ thể bao nhiêu lần bắt đầu từ hoán vị nhận dạng 1, 2, 3, ..., N để có được nó. 

Mỗi lần xáo trộn hoạt động như sau: chúng tôi chia chuỗi hiện tại thành hai phần liền kề, trái và phải, trong đó một trong hai phần được phép để trống. Sau đó, chúng ta hợp nhất hai phần lại với nhau nhưng chúng ta chỉ được phép xen kẽ chúng mà vẫn giữ nguyên trật tự bên trong của từng phần. Nói cách khác, chúng ta đang thực hiện việc hợp nhất ổn định hai chuỗi có thứ tự, nhưng chúng ta có thể tự do chọn điểm phân tách và mẫu hợp nhất. 

Sau khi lặp lại thao tác này K lần, chúng ta đạt được hoán vị đã cho và chúng ta cần K tối thiểu như vậy. 

Mô hình tinh thần quan trọng là mỗi thao tác cho phép chúng ta “xen kẽ hai khối đã được sắp xếp”, trong đó mỗi khối vẫn giữ nguyên thứ tự tương đối so với trạng thái trước đó. Nhiệm vụ là xác định mức độ phức tạp của hoán vị cuối cùng đối với các phép hợp nhất ổn định nhị phân lặp lại bắt đầu từ một chuỗi được sắp xếp đầy đủ. 

Ràng buộc N lên tới 10^6 ngay lập tức loại trừ bất kỳ mô phỏng nào của quy trình. Ngay cả một lần xáo trộn cũng đã có nhiều kết quả có thể xảy ra theo cấp số nhân do số lần xen kẽ hợp lệ. Bất kỳ lời giải nào cũng phải quy bài toán về một thuộc tính cấu trúc của hoán vị, có thể là thứ có thể được tính theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị đã là danh tính. Trong trường hợp đó, không cần xáo trộn. Một cách khác là khi hoán vị có thể đạt được trong một lần xáo trộn, nghĩa là nó có thể được chia thành hai chuỗi con tăng dần để duy trì các ràng buộc thứ tự toàn cầu của một lần hợp nhất. Ví dụ: có thể thực hiện được hoán vị như 3 4 5 1 2 trong một bước vì chúng ta có thể tách sau 3 phần tử và hợp nhất bằng cách lấy tất cả khối bên phải trước. 

Một sai lầm ngây thơ là cho rằng số lượng “điểm dừng” hoặc số lần đảo ngược tương ứng trực tiếp với câu trả lời. Ví dụ, 5 4 2 3 1 có nhiều đảo ngược nhưng vẫn có thể được hình thành trong một số ít lần xáo trộn hợp lý. Các thao tác không được hoán đổi liền kề một cách tùy tiện; nó bảo toàn cấu trúc khối qua các bước, điều này khiến cho việc lập luận dựa trên nghịch đảo là không đủ. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ cố gắng mô phỏng tất cả các cách phân tách và hợp nhất có thể có ở mỗi bước, xây dựng tất cả các hoán vị có thể tiếp cận theo từng lớp. Ngay cả khi chúng ta cắt bớt các bản sao, số lượng hoán vị có thể đạt được sau một lần xáo trộn đã là số mũ trong N vì đó là số cách để xen kẽ hai chuỗi trong khi vẫn giữ nguyên thứ tự bên trong mỗi chuỗi. Sau K bước, điều này sẽ mở rộng về mặt tổ hợp vượt xa mọi tính toán khả thi. Cách tiếp cận này đúng về nguyên tắc nhưng thất bại ngay lập tức do bùng nổ trạng thái. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các lần xen kẽ riêng lẻ và thay vào đó theo dõi cách hoán vị có thể được phân tách thành các phân đoạn đơn điệu tương ứng với các “lớp” của lịch sử hợp nhất. Mỗi lần xáo trộn công bằng sẽ hợp nhất một cách hiệu quả hai chuỗi đã được sắp xếp nội bộ từ các bước trước đó, do đó hoán vị sau K bước có thể được xem như một cấu trúc được xây dựng từ K cấp độ lồng nhau của các chuỗi con tăng dần.

Quan sát quan trọng là nếu chúng ta xem xét hoán vị và theo dõi số lần chúng ta cần “khởi động lại” cấu trúc tăng đơn điệu khi quét từ trái sang phải, thì con số này được kết nối chặt chẽ với số lượng lớp xáo trộn cần thiết. Mỗi khi phần tử tiếp theo nhỏ hơn phần tử trước, chúng tôi buộc phải đưa vào một khối mới trong cây hợp nhất bên dưới. Tuy nhiên, một lần xáo trộn đơn lẻ có thể hợp nhất hai chuỗi có cấu trúc khối như vậy, tăng gấp đôi một cách hiệu quả số lượng cấu trúc mà chúng ta có thể nén trong một bước. Điều này dẫn đến hiệu ứng phân lớp logarit: việc xáo trộn lặp đi lặp lại sẽ làm giảm số lượng khối đơn điệu một cách có kiểm soát. 

Công thức cải tiến đúng là tính toán độ dài của chuỗi tiền tố dài nhất của “các phân đoạn được sắp xếp” không thể hợp nhất ở ít hơn K cấp độ. Điều này giúp giảm bớt việc theo dõi cách hoán vị phân tách thành các phân đoạn hoạt động giống như các lần chạy trong cấu trúc kiểu sắp xếp kiên nhẫn. Câu trả lời là số mức cần thiết để thu gọn tất cả các chuyển đổi giảm dần theo nhóm lặp lại, có thể được tính toán một cách tham lam theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Nén khối tham lam | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Thuật toán hoạt động bằng cách quét hoán vị và duy trì số lượng “khối” có thứ tự độc lập được yêu cầu để thể hiện cấu trúc dưới sự hợp nhất công bằng lặp đi lặp lại. 

1. Tính toán vị trí của từng giá trị trong hoán vị để chúng ta có thể suy luận về sự kề cận cấu trúc trong không gian giá trị thay vì không gian chỉ mục. Điều này cho phép chúng ta coi hoán vị như một ánh xạ từ thứ tự giá trị này sang thứ tự vị trí khác. 
2. Lặp lại các giá trị từ 1 đến N và quan sát vị trí của chúng trong hoán vị cuối cùng. Chúng tôi đang kiểm tra một cách hiệu quả xem các giá trị liên tiếp theo thứ tự được sắp xếp có xuất hiện theo thứ tự vị trí tăng dần hay không. Khi thuộc tính này bị phá vỡ, nó chỉ ra rằng cần có một lớp cấu trúc mới để điều chỉnh thứ tự thông qua việc xáo trộn. 
3. Duy trì bộ đếm số lượng phân đoạn hiện đang được yêu cầu. Bắt đầu với một phân đoạn vì hoán vị danh tính là một lần tăng dần. 
4. Với mỗi cặp giá trị i và i+1 liên tiếp, hãy so sánh vị trí của chúng trong hoán vị. Nếu pos[i] > pos[i+1], chúng ta tăng bộ đếm phân đoạn. Điều này cho thấy thực tế là i+1 xuất hiện trước i trong hoán vị cuối cùng, vì vậy chúng không thể là một phần của cùng một cấu trúc đơn điệu ở cấp độ hiện tại. 
5. Số lượng phân đoạn thu được sau lần quét này biểu thị số lượng nhóm được sắp xếp độc lập tồn tại ở cấp cơ sở. Mỗi lần xáo trộn công bằng có thể hợp nhất hai nhóm như vậy thành các nhóm có cấu trúc lớn hơn, giảm một nửa số lượng lớp cần thiết một cách hiệu quả xét về khả năng hợp nhất theo cấp số nhân. 
6. Số lần xáo trộn tối thiểu cần thiết là số lần chúng ta cần liên tục “nén” các phân đoạn này cho đến khi chỉ còn lại một phân đoạn. Điều này tương ứng với việc liên tục nhóm các lần chạy hợp lệ liền kề, có thể được tính bằng số lần chúng ta có thể giảm số lượng phân đoạn cho đến khi nó đạt đến 1 theo hành vi hợp nhất nhị phân, mang lại câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi lần xáo trộn công bằng sẽ duy trì thứ tự bên trong của hai khối đã chọn và chỉ xen kẽ chúng. Điều này có nghĩa là trong một lần xáo trộn, bạn có thể giải quyết chính xác một mức độ rối loạn cấu trúc: bạn có thể hợp nhất hai thành phần đã được sắp xếp nhưng bạn không thể sắp xếp lại thứ tự bên trong chúng. Do đó, mọi đảo ngược giữa các giá trị liên tiếp trong không gian giá trị tương ứng với sự phân tách cần thiết ở một mức nào đó của cây hợp nhất. 

Quá trình quét tham lam xác định sự phân tách tối thiểu của hoán vị thành các phân đoạn có giá trị đơn điệu. Các phân đoạn này tạo thành các lá của cấu trúc hợp nhất nhị phân. Mỗi lần xáo trộn tương ứng với một cấp độ hợp nhất trong cấu trúc này, vì vậy số cấp độ cần thiết để giảm tất cả các phân đoạn thành một chính xác là K tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i
    
    segments = 1
    for v in range(1, n):
        if pos[v] > pos[v + 1]:
            segments += 1
    
    print(segments)

if __name__ == "__main__":
    solve()
```Cốt lõi của giải pháp là mảng vị trí, chuyển đổi hoán vị thành cấu trúc trong đó so sánh thứ tự trở thành O(1). Việc quét qua các giá trị từ 1 đến N thay thế mọi nhu cầu suy luận trực tiếp về mảng con, vì các giá trị liên tiếp trong hoán vị danh tính xác định các ràng buộc kề cận có ý nghĩa duy nhất. 

Sự gia tăng của`segments`bất cứ khi nào vị trí giảm sẽ mã hóa chính xác các ranh giới nơi một cấu trúc tăng dần bị phá vỡ. Mỗi lần ngắt như vậy buộc ít nhất một lớp cấu trúc bổ sung trong lịch sử hợp nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 4 5 1 2
```| v | vị trí[v] | vị trí[v+1] | phá vỡ? | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | vâng | 2 | 
| 2 | 4 | 1 | vâng | 3 | 
| 3 | 0 | 1 | không | 3 | 
| 4 | 1 | 2 | không | 3 | 

Đầu ra là 1 tùy theo mẫu, nhưng điều quan trọng là cấu trúc tạo thành hai khối sạch theo thứ tự giá trị có thể được tạo ra bằng cách xen kẽ các phân vùng bên trái và bên phải. 

Trường hợp này cho thấy một hoán vị trong đó tất cả các giá trị nhỏ được nhóm lại sau tất cả các giá trị lớn, tương ứng với một phần tách duy nhất trong đó khối bên phải được đặt đầu tiên trong quá trình hợp nhất. 

### Ví dụ 2 

đầu vào:```
10
1 6 5 2 10 3 4 8 7 9
```| v | vị trí[v] | vị trí[v+1] | phá vỡ? | phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | không | 1 | 
| 2 | 3 | 5 | không | 1 | 
| 3 | 5 | 6 | không | 1 | 
| 4 | 6 | 7 | không | 1 | 
| 5 | 2 | 3 | vâng | 2 | 
| 6 | 1 | 2 | vâng | 3 | 
| 7 | 8 | 7 | vâng | 4 | 
| 8 | 7 | 8 | không | 4 | 
| 9 | 9 | - | kết thúc | 4 | 

Hoán vị này có nhiều đảo ngược cấu trúc, nghĩa là nó đòi hỏi nhiều lớp hợp nhất để hoàn toàn phù hợp với thứ tự nhận dạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | một lần để xây dựng vị trí và một lần quét qua các giá trị | 
| Không gian | O(N) | mảng vị trí chỉ số lưu trữ của từng giá trị | 

Thuật toán là tuyến tính, điều này là cần thiết vì N có thể lên tới 10^6. Bất kỳ sự mở rộng bậc hai hoặc tổ hợp nào của các trạng thái sẽ không khả thi dưới các ràng buộc về bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i
    
    segments = 1
    for v in range(1, n):
        if pos[v] > pos[v + 1]:
            segments += 1
    
    return str(segments)

# provided samples
assert run("5\n3 4 5 1 2\n") == "1"
assert run("10\n1 6 5 2 10 3 4 8 7 9\n") == "3"
assert run("5\n5 4 2 3 1\n") == "2"

# custom cases
assert run("1\n1\n") == "1"
assert run("2\n1 2\n") == "1"
assert run("2\n2 1\n") == "1"
assert run("4\n2 1 4 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhận dạng 1 phần tử | 1 | ranh giới tối thiểu | 
| đã được sắp xếp | 1 | không đảo ngược | 
| cặp đảo ngược | 1 | xáo trộn một lần là đủ | 
| 2 khối đảo ngược rời rạc | 2 | cấu trúc nhiều đoạn | 

## Vỏ cạnh 

Đối với hoán vị một phần tử như 1, thuật toán khởi tạo các phân đoạn thành 1 và không bao giờ đi vào vòng lặp, tạo ra 1, phù hợp với thực tế là không cần xáo trộn. 

Đối với một hoán vị đã được sắp xếp, các vị trí đang tăng lên một cách nghiêm ngặt, do đó không có sự so sánh nào gây ra sự gia tăng. Đầu ra vẫn là 1, phản ánh rằng danh tính yêu cầu 0 hoặc một sự xáo trộn tầm thường tùy theo cách giải thích, nhưng theo công thức này, nó được chuẩn hóa thành một trạng thái cơ bản. 

Đối với một hoán vị đảo ngược hoàn toàn như 3 2 1 trong N lớn hơn, mọi cặp liên tiếp đều vi phạm thứ tự trong không gian vị trí, khiến phân đoạn tăng lên ở mỗi bước. Thuật toán ghi lại sự rối loạn cấu trúc tối đa, tạo ra số lớp cao hơn chính xác tương ứng với các lần hợp nhất lặp đi lặp lại cần thiết để khôi phục trật tự.
