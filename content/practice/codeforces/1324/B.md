---
problem: 1324B
contest_id: 1324
problem_index: B
name: "Yet Another Palindrome Problem"
contest_name: "Codeforces Round 627 (Div. 3)"
rating: 1100
tags: ["brute force", "strings"]
answer: passed_samples
verified: true
solve_time_s: 181
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2df146-8624-83ec-aa29-eb24bce32801
---

#CF 1324B - Lại thêm một bài toán về Palindrome 

**Đánh giá:** 1100 
**Tags:** bạo lực, dây 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2df146-8624-83ec-aa29-eb24bce32801 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một số mảng số nguyên độc lập và với mỗi mảng, chúng ta cần quyết định xem liệu chúng ta có thể trích xuất một dãy con có độ dài ít nhất ba dãy đọc giống nhau từ trái sang phải và từ phải sang trái hay không. 

Dãy con ở đây có nghĩa là chúng ta được phép xóa các phần tử nhưng không thể sắp xếp lại những phần còn lại. Vì vậy, nhiệm vụ không phải là về các phân đoạn liền kề mà là chọn một vài phần tử trong khi vẫn giữ nguyên thứ tự ban đầu của chúng. Trong số tất cả các lựa chọn như vậy, chúng tôi muốn biết liệu có tồn tại ít nhất một palindrome có độ dài ba hoặc nhiều hơn hay không. 

Các ràng buộc là nhỏ theo nghĩa toàn cầu. Mỗi trường hợp thử nghiệm có tổng số lên tới 5000 phần tử trong tất cả các trường hợp. Điều này ngay lập tức gợi ý rằng một$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm là tốt, nhưng bất kỳ điều gì có tính khối hoặc tệ hơn trong tất cả các thử nghiệm sẽ gặp rủi ro nếu thực hiện một cách bất cẩn. Tuy nhiên, bước đột phá thực sự đến từ việc nhận ra rằng chúng ta không cần phải xây dựng các dãy con một cách rõ ràng. 

Một tư duy ngây thơ sẽ cố gắng tạo ra tất cả các chuỗi có độ dài ít nhất là ba và kiểm tra xem chuỗi nào có phải là một bảng màu hay không. Điều đó là không thể bởi vì ngay cả đối với$n = 50$, số lượng các dãy con là theo cấp số nhân. Một nỗ lực ngây thơ khác là kiểm tra tất cả các bộ ba hoặc tất cả các bộ ba có khoảng trống, nhưng điều đó sẽ bỏ sót các palindrome dài hơn. 

Cũng có những trường hợp vi tế phá vỡ trực giác tham lam. Ví dụ: trong một mảng như$[1, 2, 3, 2, 1]$, câu trả lời rõ ràng là có, nhưng chỉ tập trung vào các mẫu trùng lặp liền kề hoặc các mẫu đối xứng đơn giản sẽ không thành công vì các phần tử phù hợp bị tách rời. 

Cấu trúc ẩn chính là bất kỳ chuỗi con palindrome hợp lệ nào có độ dài ít nhất ba phải chứa giá trị lặp lại tạo thành các đầu hoặc một mẫu có thể rút gọn thành điều kiện đơn giản hơn liên quan đến các bản sao. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ quan điểm vũ phu. Một dãy palindrome con có độ dài ít nhất là ba có phần tử đầu tiên và phần tử cuối cùng bằng nhau, và phần ở giữa chính nó là một palindrome. Một cách tiếp cận trực tiếp sẽ là liệt kê tất cả các chuỗi con, xây dựng từng chuỗi con và kiểm tra xem đó có phải là một bảng màu hay không. Đây là cấp số nhân trong$n$, đại khái$O(2^n \cdot n)$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 30$. 

Chúng ta có thể giảm thiểu vấn đề một cách đáng kể bằng cách tập trung vào kết cấu thay vì xây dựng. Một chuỗi con palindrome có độ dài ít nhất là ba phải thuộc một trong hai mẫu. 

Hoặc chúng ta có một giá trị xuất hiện ít nhất ba lần, cho phép chúng ta chọn nó làm phần tử đầu tiên và cuối cùng cũng như phần tử ở giữa, tạo thành một chuỗi con như$[x, x, x]$hoặc chúng ta có thể tạo một cấu trúc đối xứng dài hơn bằng cách sử dụng hai giá trị khác nhau trong đó cặp bên ngoài khớp với nhau và có thứ gì đó tồn tại giữa chúng. 

Quan sát quan trọng là nếu chúng ta có một giá trị xuất hiện ít nhất ba lần thì câu trả lời ngay lập tức là có. Nếu không, mọi giá trị xuất hiện tối đa hai lần và cấu trúc trở nên hạn chế đến mức chúng ta chỉ cần kiểm tra xem có tồn tại giá trị xuất hiện hai lần với ít nhất một phần tử ở giữa lần xuất hiện đầu tiên và cuối cùng của nó hay không. Điều đó mang lại một dãy con palindrome có dạng$[x, y, x]$Ở đâu$x$xuất hiện hai lần và$y$nằm giữa họ. 

Điều này làm giảm toàn bộ vấn đề về việc kiểm tra tần suất và vị trí, tránh bất kỳ sự liệt kê tiếp theo nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Kiểm tra tần số + vị trí |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi quét mảng và lưu trữ vị trí của từng giá trị. Điều này cho phép chúng tôi suy luận về sự trùng lặp và khoảng cách. 
2. Nếu bất kỳ giá trị nào xuất hiện ít nhất ba lần, chúng tôi sẽ ngay lập tức trả về "CÓ". Điều này hiệu quả vì chúng ta có thể chọn lần xuất hiện thứ nhất, thứ hai và thứ ba để tạo thành một dãy con palindrome có độ dài ba. 
3. Nếu không có giá trị nào xuất hiện 3 lần thì chúng ta chỉ có giá trị xuất hiện một hoặc hai lần. 
4. Đối với mỗi giá trị xuất hiện chính xác hai lần, chúng tôi kiểm tra xem hai lần xuất hiện đó có liền kề nhau hay không. Nếu có ít nhất một chỉ mục giữa chúng, chúng ta có thể chọn hai phần tử bằng nhau đó làm phần cuối của một dãy con. 
5. Sau đó, chúng tôi kiểm tra xem có tồn tại bất kỳ phần tử nào ở giữa mà chúng tôi có thể sử dụng làm phần giữa của bảng màu hay không. Vì bất kỳ phần tử nào cũng đóng vai trò ở giữa nên điều kiện đơn giản là hai lần xuất hiện cách nhau ít nhất một chỉ mục. 
6. Nếu có bất kỳ cặp nào như vậy tồn tại, chúng tôi trả về "CÓ". 
7. Nếu không có điều kiện nào xảy ra, chúng tôi trả về "KHÔNG". 

Bước quan trọng là nhận ra rằng một khi các bản sao tồn tại với sự phân tách, thì bất kỳ phần tử trung gian nào cũng có thể đóng vai trò là trung tâm, đảm bảo một chuỗi con có độ dài ba palindrome. 

### Tại sao nó hoạt động 

Bất kỳ dãy con palindrome nào có độ dài ít nhất là ba phải có phần tử đầu tiên và phần tử cuối cùng bằng nhau. Vì vậy, chúng tôi luôn tìm kiếm một giá trị lặp lại có thể dùng làm điểm cuối. Nếu một giá trị xuất hiện ba lần trở lên, chúng ta có thể xây dựng một bảng màu hợp lệ một cách tầm thường. Nếu nó xuất hiện chính xác hai lần thì hai lần xuất hiện đó sẽ xác định điểm cuối duy nhất có thể có cho giá trị đó. Phần tử ở giữa hợp lệ tồn tại tự động vì chúng ta được phép chọn bất kỳ phần tử nào giữa chúng mà vẫn giữ nguyên thứ tự. Hạn chế về cấu trúc này làm cạn kiệt tất cả các cấu trúc có thể có, vì vậy việc kiểm tra tần số và khoảng trống sẽ mô tả đầy đủ câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = {}
        for i, x in enumerate(a):
            pos.setdefault(x, []).append(i)

        ok = False

        for x, idxs in pos.items():
            if len(idxs) >= 3:
                ok = True
                break

        if ok:
            print("YES")
            continue

        for x, idxs in pos.items():
            if len(idxs) == 2:
                i, j = idxs
                if j - i > 1:
                    ok = True
                    break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Trước tiên, mã sẽ thu thập vị trí của từng giá trị để chúng ta có thể suy luận về các mẫu lặp lại. Vòng lặp đầu tiên phát hiện trường hợp tầm thường có ba lần xuất hiện có cùng giá trị. Vòng lặp thứ hai xử lý trường hợp tồn tại chính xác hai lần xuất hiện và đảm bảo chúng không liền kề nhau. điều kiện`j - i > 1`đảm bảo có ít nhất một phần tử giữa chúng, điều này cho phép hình thành một dãy con có độ dài ba palindrome. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3
1 2 1
```Chúng tôi theo dõi các lần xuất hiện: 1 xuất hiện tại vị trí [0, 2], 2 tại [1]. Không có giá trị nào xuất hiện ba lần. Giá trị 1 xuất hiện hai lần và khoảng cách giữa vị trí 0 và 2 không trống, vì vậy chúng ta có thể chọn dãy con [1, 2, 1]. Điều này xác nhận một palindrome hợp lệ. 

| Giá trị | Vị trí | Đếm ba | Kiểm tra khoảng cách | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [0, 2] | 2 | khoảng cách tồn tại | CÓ | 

Điều này cho thấy cách một phần tử ở giữa cho phép một chuỗi con palindrome hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```Mỗi giá trị xuất hiện một lần nên không tồn tại điểm cuối lặp lại. Nếu không có điểm cuối lặp lại, không thể hình thành chuỗi con palindrome có độ dài ít nhất ba. 

| Giá trị | Vị trí | Tần số | Quyết định | 
| --- | --- | --- | --- | 
| 1 | [0] | 1 | KHÔNG | 
| 2 | [1] | 1 | KHÔNG | 
| 3 | [2] | 1 | KHÔNG | 
| 4 | [3] | 1 | KHÔNG | 
| 5 | [4] | 1 | KHÔNG | 

Điều này xác nhận rằng sự lặp lại là cần thiết cho bất kỳ công trình xây dựng hợp lệ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi mảng được quét một lần để xây dựng vị trí và sau đó được kiểm tra một lần | 
| Không gian |$O(n)$| Lưu trữ danh sách chỉ mục cho từng giá trị riêng biệt | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm tối đa là 5000, vì vậy giải pháp tuyến tính này dễ dàng đủ nhanh trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = {}
        for i, x in enumerate(a):
            pos.setdefault(x, []).append(i)

        ok = False

        for x, idxs in pos.items():
            if len(idxs) >= 3:
                ok = True
                break

        if not ok:
            for x, idxs in pos.items():
                if len(idxs) == 2 and idxs[1] - idxs[0] > 1:
                    ok = True
                    break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
3
1 2 1
5
1 2 2 3 2
3
1 1 2
4
1 2 2 1
10
1 1 2 2 3 3 4 4 5 5
""") == """YES
YES
NO
YES
NO"""

# all equal
assert run("""1
5
7 7 7 7 7
""") == "YES"

# no repeats
assert run("""1
4
1 2 3 4
""") == "NO"

# separated pair
assert run("""1
4
1 2 1 3
""") == "YES"

# adjacent duplicates only
assert run("""1
4
1 1 2 3
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | phím tắt lặp lại ba lần | 
| không lặp lại | KHÔNG | không thể không trùng lặp | 
| cặp tách biệt | CÓ | trường hợp trùng lặp không liền kề | 
| trùng lặp liền kề | KHÔNG | xây dựng liền kề nghỉ dài-3 | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một giá trị xuất hiện chính xác hai lần nhưng các lần xuất hiện lại liền kề nhau. Ví dụ, trong`[1, 1, 2]`, không có khoảng trống để đặt phần tử ở giữa giữa hai số 1, do đó không thể hình thành chuỗi con palindrome có độ dài-3 bằng cách sử dụng giá trị đó. 

Một trường hợp khác là khi nhiều giá trị xuất hiện hai lần nhưng tất cả các lần xuất hiện đều là cặp liền kề. Ví dụ`[1, 1, 2, 2, 3, 3]`có nhiều bản sao, nhưng không cái nào trong số chúng tạo ra khoảng trống, vì vậy câu trả lời vẫn là không. Thuật toán từ chối chính xác điều này vì mọi cặp đều không đạt được điều kiện phân tách. 

Trường hợp thứ ba là khi một giá trị xuất hiện ba lần nhưng nằm rải rác, chẳng hạn như`[1, 2, 1, 3, 1]`. Mặc dù các lần xuất hiện không liên tiếp, việc chọn lần xuất hiện thứ nhất, thứ ba và thứ năm vẫn tạo thành một chuỗi con palindrome hợp lệ, do đó thuật toán chỉ chấp nhận chính xác ngay lập tức trên tần số.
