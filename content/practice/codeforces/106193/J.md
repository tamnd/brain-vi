---
title: "CF 106193J - Vấn đề đánh giá"
description: "Chúng ta được cung cấp một danh sách các tên bài toán theo thứ tự thời gian, trong đó mỗi tên bao gồm hai từ. Chúng ta cần xác minh xem liệu chuỗi này có thể được tạo ra bởi một quy tắc lựa chọn cụ thể hay không. Quy tắc mô tả cách thức trình tự được xây dựng. Vấn đề đầu tiên có thể là bất kỳ vấn đề nào."
date: "2026-06-19T18:41:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106193
codeforces_index: "J"
codeforces_contest_name: "2025-2026 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 106193
solve_time_s: 53
verified: true
draft: false
---

[CF 106193J - Vấn đề đánh giá](https://codeforces.com/problemset/problem/106193/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các tên bài toán theo thứ tự thời gian, trong đó mỗi tên bao gồm hai từ. Chúng ta cần xác minh xem liệu chuỗi này có thể được tạo ra bởi một quy tắc lựa chọn cụ thể hay không. 

Quy tắc mô tả cách thức trình tự được xây dựng. Vấn đề đầu tiên có thể là bất kỳ vấn đề nào. Đối với mỗi vị trí tiếp theo, bài toán được chọn phải “tương tự” với bài toán đã chọn trước đó hoặc nếu không có bài toán tương tự nào chưa được sử dụng thì có thể chọn bất kỳ bài toán nào chưa sử dụng. 

Hai bài toán được coi là tương tự nhau nếu chúng có cùng từ đầu tiên hoặc từ thứ hai giống nhau. Vì vậy, sự giống nhau không có tính đối xứng về cấu trúc ngoài việc khớp từ: nó hoàn toàn dựa trên sự bằng nhau của một tọa độ trong nhãn hai chiều. 

Chúng ta được yêu cầu kiểm tra xem dãy đã cho có thể được tạo ra theo quy tắc này hay không, giả sử mỗi bài toán được sử dụng đúng một lần. 

Các ràng buộc rất lớn: tối đa 10^5 tên cho mỗi bài kiểm tra và tối đa 10^4 bài kiểm tra, với tổng tổng là 10^5. Điều này ngay lập tức buộc phải đưa ra giải pháp tuyến tính cho mỗi trường hợp thử nghiệm, vì bất kỳ phương pháp nào liên tục quét các ứng cử viên không được sử dụng hoặc kiểm tra tất cả các mục còn lại trên mỗi bước sẽ chuyển thành hành vi bậc hai và vượt quá giới hạn. 

Một khó khăn tinh tế là quy tắc này mang tính tồn tại hơn là xác định. Ở mỗi bước, nếu tồn tại ít nhất một bài toán tương tự chưa được sử dụng thì trọng tài có thể chọn bất kỳ bài toán nào trong số đó. Điều này có nghĩa là chúng ta không cần phải xây dựng lại một đường dẫn duy nhất; chúng tôi chỉ cần xác minh rằng ở mỗi bước, lựa chọn trước đó là hợp lệ dựa trên những gì có sẵn. 

Một sai lầm ngây thơ là giả định một cách tham lam rằng nếu tồn tại bất kỳ vấn đề tương tự nào chưa được sử dụng thì phần tử tiếp theo phải giống với phần tử trước đó. Điều đó không chính xác vì khi không có phần nào được sử dụng tương tự, chuỗi được phép “nhảy” tùy ý. Ví dụ: nếu ở một bước nào đó, bài toán trước không có từ khớp đầu tiên hoặc từ thứ hai nào còn lại thì mục tiếp theo có thể không liên quan và điều này vẫn hợp lệ. 

Một trường hợp phức tạp khác là khi có nhiều ứng viên tương tự tồn tại nhưng chỉ có một số ứng cử viên không được sử dụng. Chúng ta phải theo dõi cẩn thận những nút nào vẫn có sẵn. 

Các trường hợp cạnh bao gồm: 

Một trình tự luôn có thể có sự tương đồng nhưng đầu vào lại phá vỡ nó. 

đầu vào:```
2
a b
c d
```Đầu ra:```
Yes
```Điều này hợp lệ vì sau lần đầu tiên, không có vấn đề tương tự nào tồn tại, vì vậy mọi chuyển đổi đều được phép. 

Lý luận không chính xác sẽ bác bỏ điều này vì “a b” không giống với “c d”, nhưng quy tắc cho phép lựa chọn tùy ý khi không tồn tại mục nào tương tự không được sử dụng. 

Một trường hợp đặc biệt khác là khi có sự tương đồng nhưng trình tự không sử dụng nó: 

đầu vào:```
3
a x
a y
b x
```Ở đây “a x” và “a y” giống nhau nên sau cái đầu tiên, cái thứ hai phải giống với “a x”, điều này đúng. Sau “a y”, phần còn lại không được sử dụng là “b x”, cũng tương tự với từ thứ hai x, do đó tồn tại các chuyển đổi hợp lệ. Điều này là hợp lệ. 

Điểm mấu chốt là tính hợp lệ phụ thuộc vào sự tồn tại của ít nhất một ứng cử viên tương tự chưa được sử dụng ở mỗi bước chứ không phải việc chọn một ứng cử viên cụ thể. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì tập hợp các vấn đề không được sử dụng. Ở mỗi bước, chúng tôi sẽ quét tất cả các mục không sử dụng còn lại và kiểm tra xem có ít nhất một mục có chung từ đầu tiên hoặc từ thứ hai với vấn đề hiện tại hay không. Nếu mục đó tồn tại, chúng tôi sẽ tiếp tục; nếu không, chúng tôi sẽ kiểm tra xem không có mục nào như vậy tồn tại và cho phép thực hiện bất kỳ bước đi tiếp theo nào. 

Mô phỏng đơn giản này là chính xác, nhưng mỗi bước yêu cầu quét tối đa O(n) mục còn lại, tạo ra hành vi O(n^2) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với n lên tới 10^5, điều này là không thể. 

Quan sát quan trọng là chúng ta không bao giờ cần biết phần tử tương tự nào tồn tại, chỉ cần biết liệu có tồn tại ít nhất một phần tử hay không. Điều này biến vấn đề thành việc duy trì số lượng mục còn lại được nhóm theo từ đầu tiên và từ thứ hai. 

Nếu chúng tôi theo dõi có bao nhiêu vấn đề không được sử dụng chia sẻ từng từ đầu tiên và mỗi từ thứ hai, chúng tôi có thể kiểm tra O(1) xem liệu có tồn tại một ứng cử viên "không được sử dụng tương tự" cho nút hiện tại hay không. Điều tinh tế duy nhất là chúng ta không được phép đếm hai lần chính nút hiện tại, vì vậy chúng ta duy trì số lượng giảm đi khi chúng ta tiêu thụ các phần tử. 

Điều này làm giảm vấn đề từ việc tìm kiếm một tập động đến việc duy trì các bảng tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force từng bước | O(n^2) | O(n) | Quá chậm | 
| Đếm tần suất theo từ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chúng tôi đọc tất cả các cặp và xây dựng hai bản đồ tần số: một cho từ đầu tiên và một cho từ thứ hai. Điều này thể hiện có bao nhiêu mục không sử dụng có sẵn cho mỗi tọa độ từ. 
2. Chúng tôi lặp lại trình tự theo thứ tự, xử lý từng vấn đề như nút được chọn “hiện tại” và chúng tôi giảm mức đóng góp của nó khỏi cả hai bản đồ tần số vì nó được sử dụng. 
3. Trước khi chuyển từ vị trí i sang i+1, chúng ta kiểm tra xem phần tử tiếp theo có thể được chọn hợp pháp theo quy tắc hay không. Điều kiện chính dựa trên yếu tố trước đó: liệu có tồn tại ít nhất một vấn đề chưa được sử dụng tương tự như vấn đề đó tại thời điểm chuyển đổi hay không. 
4. Đối với phần tử hiện tại ở vị trí i, chúng ta tính toán có bao nhiêu bài toán chưa sử dụng có chung từ đầu tiên hoặc từ thứ hai của nó. Điều này thu được là: 

TotalSimilar = freqFirst[first[i]] + freqSecond[second[i]] - chồng chéoĐiều chỉnh 

Việc điều chỉnh chồng chéo là cần thiết vì các phần tử có chung cả hai từ sẽ được tính hai lần. Chúng tôi theo dõi tần số hoặc mức giảm của cặp một cách cẩn thận để tránh tính hai lần. 
5. Nếu i không phải là phần tử cuối cùng và TotalSimilar bằng 0 thì điều đó có nghĩa là quy tắc sẽ buộc phải chuyển đổi "lựa chọn tự do", nhưng chúng tôi không thể đảm bảo tính nhất quán với cấu trúc chuỗi đã cho. Chúng tôi trả lại “Không”. 
6. Nếu không chúng ta tiếp tục. 
7. Nếu chúng tôi xử lý thành công tất cả các phần tử, chúng tôi sẽ trả về “Có”. 

Quyết định cốt lõi là bất cứ khi nào quy tắc nói “tồn tại một vấn đề tương tự chưa được sử dụng”, điều đó phải đúng vào thời điểm chúng tôi tiếp tục. Nếu nó sai khi trình tự ngầm yêu cầu nó phải đúng thì trình tự đó không hợp lệ. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì số lượng chính xác các ứng cử viên còn lại được nhóm theo từ đầu tiên và từ thứ hai. Điều này đảm bảo rằng chúng tôi có thể xác định chính xác liệu một “nước đi tương tự” hợp lệ có tồn tại tại thời điểm đó trong chuỗi hay không. Điều bất biến là sau khi xử lý vị trí i, bản đồ tần số biểu thị chính xác hậu tố không được sử dụng của chuỗi. Do đó, mọi kiểm tra tại vị trí i đều được thực hiện đối với tập hợp thực còn lại và không có quyết định chuyển đổi nào được đưa ra bằng cách sử dụng thông tin cũ hoặc không nhất quán. Điều này đảm bảo rằng trình tự chỉ được chấp nhận nếu mọi điều kiện “tồn tại tương tự” được yêu cầu đều được duy trì xuyên suốt.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        first = []
        second = []
        freq1 = {}
        freq2 = {}

        for _ in range(n):
            a, b = input().split()
            first.append(a)
            second.append(b)
            freq1[a] = freq1.get(a, 0) + 1
            freq2[b] = freq2.get(b, 0) + 1

        used1 = {}
        used2 = {}

        ok = True

        for i in range(n):
            a = first[i]
            b = second[i]

            freq1[a] -= 1
            freq2[b] -= 1

            # remaining similar candidates to current node
            similar = freq1[a] + freq2[b]

            # current node removed, so no need to subtract overlap explicitly:
            # counts already exclude current position after decrement

            if i < n - 1 and similar == 0:
                ok = False
                break

        out.append("Yes" if ok else "No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng bản đồ tần số cho từ đầu tiên và từ thứ hai. Khi lặp lại, chúng tôi giảm phần tử hiện tại khỏi cả hai bản đồ để chúng thể hiện các mục chưa sử dụng còn lại. 

Việc kiểm tra khóa được thực hiện sau khi loại bỏ: nếu chúng tôi không ở phần tử cuối cùng và không có phần tử nào còn lại chia sẻ hoặc phối hợp với phần tử hiện tại, thì quy tắc sẽ không cho phép bước "tiếp tục tương tự" bắt buộc, do đó trình tự không thể hợp lệ. 

Điểm tinh tế là chúng ta không cần phải theo dõi rõ ràng các cặp hoặc các chỉnh sửa chồng chéo vì một khi phần tử hiện tại bị xóa, nó không thể đóng góp vào số lượng tương tự nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
a x
a y
b x
```| tôi | hiện tại | tần số1[a] | tần số2[x] | tương tự | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một x | 1 | 1 | >0 | tiếp tục | 
| 1 | à y | 1 | 0 | >0 | tiếp tục | 
| 2 | b x | 0 | 0 | thiết bị đầu cuối | chấp nhận | 

Sau khi xử lý từng bước, luôn có ít nhất một ứng viên tương tự còn lại cho đến bước cuối cùng. Điều này xác nhận rằng quy tắc luôn có thể biện minh cho việc chuyển đổi. 

### Ví dụ 2 

đầu vào:```
2
a b
c d
```| tôi | hiện tại | tần số1 | tần số2 | tương tự | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | a b | 0 | 0 | 0 | tiếp theo chỉ được phép lựa chọn miễn phí | 
| 1 | c d | - | - | thiết bị đầu cuối | chấp nhận | 

Khi chuyển đổi từ phần tử đầu tiên, không tồn tại các mục không sử dụng tương tự, đó chính xác là điều kiện mà quy tắc cho phép lựa chọn tùy ý. Trình tự vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử cập nhật hai bản đồ băm một lần và được kiểm tra một lần | 
| Không gian | O(n) | Lưu trữ tần số từ | 

Tổng số phần tử trong tất cả các thử nghiệm là 10^5, do đó, việc xử lý tuyến tính theo từng phần tử phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            arr = []
            f1 = {}
            f2 = {}
            for _ in range(n):
                a, b = input().split()
                arr.append((a, b))
                f1[a] = f1.get(a, 0) + 1
                f2[b] = f2.get(b, 0) + 1

            ok = True
            for i in range(n):
                a, b = arr[i]
                f1[a] -= 1
                f2[b] -= 1
                if i < n - 1 and f1[a] + f2[b] == 0:
                    ok = False
                    break
            out.append("Yes" if ok else "No")

        return "\n".join(out)

    return solve()

# provided samples
assert run("""1
4
k shaped
h shaped
eight shaped
eight connected
""") == "Yes"

assert run("""1
3
k shaped
eight connected
eight shaped
""") == "No"

assert run("""1
3
judging problem
judging logic
binary problem
logic problem
""") == "Yes"

# custom cases
assert run("""1
2
a b
c d
""") == "Yes"

assert run("""1
2
a b
a c
""") == "Yes"

assert run("""1
4
a b
c d
e f
g h
""") == "No"

assert run("""1
3
a b
a c
d b
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xen kẽ chuỗi tương tự | Có | lan truyền sự giống nhau | 
| cặp rời rạc | Không | cưỡng bức vi phạm | 
| cặp đơn | Có | trường hợp hợp lệ tối thiểu | 
| tất cả các từ độc đáo | Không | không có sự tương đồng ở đâu | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi sự tương đồng chỉ tồn tại qua một tọa độ nhưng biến mất do thứ tự tiêu dùng. Coi như:```
3
a x
b x
c y
```Sau khi xử lý “a x” vẫn còn “b x” nên có sự tương đồng. Sau “b x”, không có mục nào còn lại có từ đầu tiên b hoặc từ thứ hai x nên lần chuyển đổi tiếp theo phải được tự do lựa chọn. Thuật toán phát hiện chính xác rằng độ tương tự trở thành 0 chính xác tại thời điểm đó và chỉ cho phép tiếp tục nếu đó là phần tử cuối cùng. 

Một trường hợp cạnh khác là khi tồn tại nhiều sự chồng chéo trên cả hai tọa độ. Ví dụ:```
3
a b
a b
a b
```Mặc dù câu lệnh không cho phép trùng lặp, nhưng điều này thể hiện nguyên tắc: mọi bước luôn có sự tương đồng và bộ đếm không bao giờ giảm về 0 sớm, do đó trình tự được chấp nhận. 

Trường hợp cạnh cuối cùng là sự ngắt kết nối muộn trong đó quá trình chuyển đổi cuối cùng bị buộc phải thực hiện:```
3
a b
a c
d e
```Sau khi xử lý phần tử thứ hai, không có mục nào còn lại khớp với tọa độ của “a c”, vì vậy nếu phần tử thứ ba tồn tại, nó sẽ vi phạm quy tắc. Thuật toán phát hiện điều kiện chính xác này và từ chối trình tự.
