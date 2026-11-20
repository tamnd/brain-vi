---
title: "CF 2178A - Có hoặc Có"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm hai ký hiệu, trong đó một ký hiệu hoạt động giống như một giá trị trung tính và ký hiệu kia hoạt động giống như một giá trị “thực” hấp thụ trong một hoạt động hợp nhất giống OR."
date: "2026-06-07T22:21:37+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2025"
rating: 800
weight: 2178
solve_time_s: 79
verified: true
draft: false
---

[CF 2178A – Có hoặc Có](https://codeforces.com/problemset/problem/2178/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm hai ký hiệu, trong đó một ký hiệu hoạt động giống như một giá trị trung tính và ký hiệu kia hoạt động giống như một giá trị “thực” hấp thụ trong một hoạt động hợp nhất giống OR. Thao tác cho phép chúng ta chọn hai ký tự liền kề, xóa chúng và thay thế bằng logic OR của chúng: nếu một trong hai ký tự là Y, kết quả là Y, nếu không thì là N. Mỗi thao tác rút ngắn chuỗi đúng một ký tự và chúng ta tiếp tục lặp lại điều này cho đến khi chỉ còn lại một ký tự. 

Hạn chế là chúng tôi không bao giờ được phép hợp nhất trực tiếp hai ký tự Y. Việc di chuyển đó bị cấm mặc dù thông thường nó vẫn tạo ra Y. Nhiệm vụ là quyết định xem có tồn tại một số chuỗi hợp nhất làm giảm chuỗi thành một ký tự mà không bao giờ chọn cặp YY hay không. 

Các ràng buộc rất nhỏ: mỗi chuỗi có độ dài tối đa là 100 và có tới 500 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu lập trình động phức tạp trên các trạng thái lớn hoặc tìm kiếm trên tất cả các thứ tự hợp nhất. Về mặt lý thuyết, thậm chí có thể hình dung được việc khám phá theo cấp số nhân của tất cả các chuỗi hợp nhất ở mức n = 100, nhưng ràng buộc bổ sung cấm YY hợp nhất sẽ cắt bớt cấu trúc đủ mạnh để có thể mong đợi một đặc tính tổ hợp trực tiếp. 

Một điểm tinh tế quan trọng là việc hợp nhất sẽ thay đổi các mối quan hệ liền kề. Một cách giải thích ngây thơ có thể cho rằng chúng ta chỉ có thể đếm Y và N, nhưng tính kề cận rất quan trọng vì Y có thể trở thành hàng xóm sau khi xóa. Một cạm bẫy tinh vi khác là nghĩ rằng việc tránh hợp nhất YY có nghĩa là Y phải tách biệt mãi mãi, điều này không đúng vì Y có thể di chuyển “qua” N hợp nhất một cách gián tiếp. 

Ví dụ, trong`YNY`, có thể kết hợp`NY -> Y`cho đi`YY`, vốn đã không hợp lệ nếu chúng ta phải hợp nhất chúng trực tiếp sau đó. Nhưng trong một số cấu hình, việc sắp xếp cẩn thận sẽ ngăn cản việc phải hợp nhất trực tiếp hai Y ngay cả khi tồn tại nhiều Y. 

Vì vậy, câu hỏi thực sự là liệu chúng ta có thể lên lịch hợp nhất để không có bước nào tạo ra sự liền kề YY tại thời điểm hợp nhất hay không. 

## Phương pháp tiếp cận 

Ý tưởng Brute Force là mô phỏng tất cả các cách có thể để giảm chuỗi bằng cách liên tục chọn các cặp liền kề để hợp nhất, đồng thời theo dõi xem liệu hợp nhất YY có từng được sử dụng hay không. Điều này tạo thành một không gian trạng thái trong đó mỗi trạng thái là một chuỗi và các chuyển đổi tương ứng với việc hợp nhất bất kỳ cặp liền kề nào. Số lượng các chuỗi hợp nhất có thể có là rất lớn, vì một chuỗi có độ dài n ban đầu có khoảng n lựa chọn, sau đó là n−1, v.v., dẫn đến tăng trưởng giai thừa. Mặc dù n chỉ là 100 nhưng con số này quá lớn để khám phá. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các chuỗi trung gian một cách chi tiết. Sự kiện nguy hiểm duy nhất là khi hai chữ Y trở nên liền kề vào thời điểm chúng ta quyết định hợp nhất chúng. Điều này gợi ý rằng Y hoạt động giống như các chướng ngại vật phải cách nhau ít nhất một N ở mọi giai đoạn mà chúng có thể trở thành hàng xóm của nhau. 

Bây giờ hãy xem N làm gì. Việc hợp nhất bất cứ thứ gì liên quan đến N có xu hướng loại bỏ N hoặc “hấp thụ” nó vào Y. Điều này có nghĩa là N không phải là rào cản ổn định. Tuy nhiên, một sự đơn giản hóa cấu trúc quan trọng xuất hiện: nếu có ít nhất hai chữ Y trong chuỗi thì tại một thời điểm nào đó trong bất kỳ lần rút gọn nào, những chữ Y đó chắc chắn sẽ trở nên liền kề trong bất kỳ lần rút gọn một ký tự cuối cùng nào trừ khi có ít nhất một chữ N để phân tách chúng trong suốt quá trình. Nhưng N đã bị tiêu hao nên chúng không thể tách vĩnh viễn nhiều Y. 

Điều này dẫn đến sự phân đôi mạnh mẽ: nếu có hai chữ Y trở lên, câu trả lời thường là KHÔNG, ngoại trừ trường hợp cấu trúc cho phép chúng ta thu gọn mọi thứ mà không cần hợp nhất YY. Điều đó hóa ra là không thể khi có ít nhất hai Y vì bất kỳ quá trình co nào làm giảm độ dài xuống một cuối cùng đều phải hợp nhất qua một ranh giới buộc Y liền kề. 

Các trường hợp an toàn duy nhất còn sống sót là: 

khi không có Y, hoặc chính xác một Y với các N xung quanh được sắp xếp sao cho Y không bao giờ gặp Y khác trong quá trình co lại. Trên thực tế, điều này còn đơn giản hóa hơn nữa thành một điều kiện rõ ràng: chuỗi hợp lệ nếu nó chứa nhiều nhất một Y. 

Chúng ta có thể biện minh cho điều này bằng cách kiểm tra xem: 

nếu không có Y nào, thì mỗi lần hợp nhất là NN → N, do đó chuỗi luôn giữ nguyên toàn bộ N và có thể rút gọn một cách tầm thường. 

nếu có chính xác một Y, thì mọi sự hợp nhất liên quan đến nó với N đều tạo ra Y và vì không có Y thứ hai tồn tại nên không thể xảy ra sự hợp nhất YY bị cấm. 

nếu có hai hoặc nhiều Y, thì bất kỳ việc rút gọn nào thành một ký tự cuối cùng đều yêu cầu kết hợp các vùng chứa Y, điều này buộc phải có sự kề cận của YY ở một số bước trong bất kỳ chuỗi hợp nhất nào. 

Vì vậy bài toán rút gọn về điều kiện đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của tất cả các đơn hàng hợp nhất | Ồ (n!) | O(n) | Quá chậm | 
| Đếm Y và kiểm tra xem ≤ 1 | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy quét chuỗi và đếm xem có bao nhiêu ký tự bằng Y. Điều này sẽ nắm bắt tất cả thông tin liên quan đến việc liệu có thể ép buộc việc hợp nhất bị cấm hay không. 
2. Nếu số đếm của Y bằng 0, ngay lập tức xuất CÓ. Không có thao tác nào có thể tạo ra Y, do đó chuỗi hoạt động như một chuỗi thu gọn hoàn toàn của N giá trị. 
3. Nếu số lượng của Y chính xác là một, xuất ra CÓ. Bất kỳ sự hợp nhất nào liên quan đến Y đơn lẻ đó không bao giờ có thể liên quan đến Y khác, do đó thao tác bị cấm không bao giờ được kích hoạt. 
4. Nếu số lượng Y là hai hoặc nhiều hơn, xuất ra NO. Với sự hiện diện của nhiều Y, bất kỳ việc rút gọn hoàn toàn nào thành một ký tự nhất thiết phải đưa Y vào trạng thái liền kề ở một giai đoạn trung gian nào đó, khiến cho việc hợp nhất bị cấm là không thể tránh khỏi. 

### Tại sao nó hoạt động

Cách duy nhất để tạo ra Y trong quá trình hoạt động là hợp nhất với ít nhất một Y. Khi tồn tại nhiều Y, mọi bước rút gọn sẽ bảo toàn hoặc giảm số lượng của chúng, nhưng không thể tách chúng thành các thành phần cách ly vĩnh viễn vì việc hợp nhất sẽ loại bỏ ranh giới. Vì quá trình này phải giảm chuỗi thành một ký tự duy nhất nên cuối cùng tất cả các vùng đều tương tác với nhau. Điều đó đảm bảo một giai đoạn trong đó hai vùng gốc Y trở nên liền kề và cần được hợp nhất trực tiếp, vi phạm ràng buộc. Với tối đa một Y, không tồn tại sự tương tác như vậy giữa các vùng gốc Y, do đó quá trình luôn có thể tiến hành một cách an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    y_count = s.count('Y')
    if y_count <= 1:
        print("YES")
    else:
        print("NO")
```Việc triển khai áp dụng trực tiếp quan sát rằng chỉ có số lượng ký tự Y mới quan trọng. các`.count()`thao tác quét chuỗi một lần cho mỗi trường hợp thử nghiệm, điều này là đủ với các ràng buộc. 

Điểm tinh tế duy nhất là đảm bảo chúng tôi xử lý tất cả các trường hợp một cách thống nhất: cả chuỗi N trội và chuỗi Y đơn đều được chấp nhận, trong khi mọi số lượng Y lớn hơn đều bị từ chối. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1:`NNNY`| Bước | Chuỗi | đếm Y | Quyết định | 
| --- | --- | --- | --- | 
| 0 | NNNY | 1 | tiếp tục | 
| cuối cùng | tồn tại một Y | 1 | CÓ | 

Điều này cho thấy rằng một Y đơn lẻ không bao giờ bắt buộc phải tương tác với một Y khác và tất cả các phép hợp nhất luôn có thể liên quan đến tối đa một Y cho mỗi thao tác. 

### Ví dụ 2:`YYNY`| Bước | Chuỗi | đếm Y | Quan sát | 
| --- | --- | --- | --- | 
| 0 | YYNY | 3 | hiện tại của nhiều Y | 
| cuối cùng | - | - | không thể tránh khỏi tương tác Y-Y | 

Trường hợp này chứng minh rằng nhiều vùng Y chắc chắn sẽ sụp đổ thành vùng lân cận trong bất kỳ quá trình giảm hoàn toàn nào, buộc phải hợp nhất bị cấm. 

Bất biến được minh họa ở đây là số lượng thành phần gốc Y không thể được giữ tách biệt thông qua việc giảm hoàn toàn độ dài một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | đếm một lần ký tự Y | 
| Không gian | O(1) | chỉ có một bộ đếm được lưu trữ | 

Các ràng buộc cho phép tối đa 500 chuỗi có độ dài 100, do đó giải pháp này chạy tốt dưới một phần nghìn giây cho mỗi trường hợp thử nghiệm và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print("YES" if s.count('Y') <= 1 else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("""7
YY
NN
NNY
YYYNY
NNNNN
YYYYYY
YNNNNN
""") == """NO
YES
YES
NO
YES
NO
YES"""

# custom cases
assert run("""3
Y
N
YN
""") == """YES
YES
YES"""

assert run("""2
YY
YYY
""") == """NO
NO"""

assert run("""1
NNNNNNNNNN
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Có/Không/Có | CÓ CÓ CÓ | ký tự đơn và trường hợp tối thiểu | 
| YY / YYY | KHÔNG KHÔNG | từ chối nhiều Y | 
| tất cả chuỗi N | CÓ | ổn định hoàn toàn trung tính | 

## Vỏ cạnh 

Một Y duy nhất ở bất kỳ vị trí nào luôn hoạt động giống nhau vì việc hợp nhất với N không tạo ra các tương tác Y mới. Ví dụ, trong`NYNN`, mọi chuỗi hợp nhất hợp lệ sẽ giữ tối đa một Y hoạt động, do đó quá trình luôn có thể tiến hành an toàn xuống một ký tự đơn lẻ mà không bao giờ hình thành liền kề YY. 

Một chuỗi như`YNNY`phơi bày phương thức thất bại của lý luận ngây thơ chỉ dựa trên sự kề cận. Mặc dù ban đầu các chữ Y được tách ra, nhưng bất kỳ chuỗi hợp nhất nào làm giảm độ dài cuối cùng sẽ tạo ra sự tương tác giữa các vùng của chúng. Tại thời điểm đó, hai thành phần có nguồn gốc từ Y phải đáp ứng, điều này sẽ yêu cầu hợp nhất YY bị cấm. Đây chính xác là lý do tại sao việc đếm Y là đủ: sự phân tách không được bảo toàn khi co lại.
