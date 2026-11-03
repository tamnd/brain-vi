---
title: "CF 1107C - Sự tàn bạo"
description: "Chúng ta được cung cấp một chuỗi các đòn đánh cố định, trong đó mỗi đòn đánh có một giá trị sát thương và được liên kết với một nút cụ thể (một chữ cái viết thường). Chúng tôi được phép xóa bất kỳ lần truy cập nào khỏi chuỗi trong khi vẫn giữ các lần truy cập còn lại theo thứ tự ban đầu."
date: "2026-06-12T05:26:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1107
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 59 (Rated for Div. 2)"
rating: 1300
weight: 1107
solve_time_s: 327
verified: false
draft: false
---

[CF 1107C - Sự tàn bạo](https://codeforces.com/problemset/problem/1107/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 5 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các đòn đánh cố định, trong đó mỗi đòn đánh có một giá trị sát thương và được liên kết với một nút cụ thể (một chữ cái viết thường). Chúng tôi được phép xóa bất kỳ lần truy cập nào khỏi chuỗi trong khi vẫn giữ các lần truy cập còn lại theo thứ tự ban đầu. Tổng sát thương là tổng số lần đánh được giữ. 

Hạn chế là nếu cùng một nút được nhấn nhiều hơn`k`nhiều lần liên tiếp trong chuỗi kết quả, gamepad sẽ bị hỏng. Vì vậy, trong dãy con được chọn cuối cùng, mọi khối tối đa của các chữ cái giống nhau phải có độ dài tối đa`k`. 

Nhiệm vụ là chọn một chuỗi con tối đa hóa tổng thiệt hại theo ràng buộc này. 

Kích thước đầu vào tăng lên`2 · 10^5`, do đó mọi nghiệm bậc hai trong`n`ngay lập tức là quá chậm. Thậm chí`O(n log n)`có thể chấp nhận được, nhưng cấu trúc của bài toán gợi ý rõ ràng rằng chúng ta cần một cách tiếp cận tham lam tuyến tính hoặc gần tuyến tính, bởi vì các quyết định chỉ phụ thuộc vào các khối cục bộ có ký tự bằng nhau chứ không phụ thuộc vào các tương tác toàn cục. 

Một ý tưởng ngây thơ là xem xét tất cả các chuỗi con, nhưng ngay cả khi giới hạn bản thân ở một ký tự duy nhất, chúng ta vẫn buộc phải xem xét các tập hợp con tổ hợp bên trong mỗi khối gồm các chữ cái bằng nhau, theo cấp số nhân trong trường hợp xấu nhất. 

Một vấn đề phức tạp hơn xuất hiện khi các yếu tố có sức sát thương cao xảy ra ở giữa các chặng đường dài. Ví dụ: nếu một chữ cái lặp lại nhiều lần và một lần xuất hiện có mức độ thiệt hại cực kỳ cao, chúng tôi có thể muốn đảm bảo rằng nó không bị loại trừ chỉ vì các lựa chọn trước đó đã lấp đầy hạn ngạch của`k`. 

Một trường hợp góc khác là khi nhiều khối dài có cùng chữ cái được phân tách bằng các chữ cái khác. Mặc dù chúng không liên tiếp trong chuỗi gốc, nhưng sau khi xóa, chúng có thể hợp nhất thành một khối duy nhất, do đó việc bỏ qua các quyết định trong các phân đoạn trước sẽ ảnh hưởng đến tính khả thi sau này. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là: chúng tôi thử mọi tập hợp con của các chỉ số, tính toán chuỗi kết quả và kiểm tra xem có bất kỳ chuỗi ký tự giống hệt nào vượt quá độ dài không`k`. Đối với các tập hợp con hợp lệ, chúng tôi tính tổng thiệt hại và lấy mức tối đa. Điều này rõ ràng là theo cấp số nhân trong`n`, vì có`2^n`tập hợp con và thậm chí việc kiểm tra từng tập hợp con cũng mất`O(n)`. 

Quan sát quan trọng là tính khả thi chỉ phụ thuộc vào việc chạy các ký tự giống hệt nhau trong _kết quả sau_ và chúng ta có thể bỏ qua các phần tử để kiểm soát độ dài chạy đó. Bên trong bất kỳ đoạn liền kề nào của các chữ cái giống hệt nhau trong chuỗi gốc, chúng tôi đang lựa chọn các phần tử cần giữ lại một cách hiệu quả nhưng thứ tự của chúng là cố định. 

Bây giờ hãy tập trung vào một chữ cái duy nhất`c`. Giả sử tất cả các vị trí ở đó`s[i] = c`được xem xét. Chúng ta muốn chọn một chuỗi con của những lần xuất hiện này, nhưng với hạn chế là trong chuỗi cuối cùng, không có chuỗi nào xảy ra.`c`vượt quá`k`. Tuy nhiên, vì các chữ cái khác bị ngắt chạy ở giữa nên việc ghép thực sự xảy ra trên mỗi khối tối đa gồm các chữ cái giống hệt nhau liên tiếp trong chuỗi gốc. 

Vì vậy chúng ta phân tách chuỗi thành các đoạn có ký tự bằng nhau. Bên trong mỗi phân đoạn, chúng ta buộc phải xử lý một chữ cái duy nhất. Ràng buộc trở thành: trong mỗi phân đoạn, chúng tôi không thể lấy nhiều hơn`k`các mục trong một hàng, nhưng vì tất cả đều giống hệt nhau, điều này đơn giản trở thành việc chọn tập hợp con tốt nhất đồng thời đảm bảo rằng chúng tôi không bao giờ chọn nhiều hơn`k`lựa chọn liên tiếp bên trong phân khúc đó. Do đó, chiến lược tối ưu trong một phân khúc rất đơn giản: chúng tôi giữ`k`giá trị lớn nhất nếu đoạn đó hoàn toàn độc lập. 

Tuy nhiên, các phân đoạn chỉ tương tác thông qua việc chúng ta có bắt đầu một lượt chạy mới có cùng ký tự hay không. Quan điểm toàn cầu đúng đắn thì đơn giản hơn: đối với mỗi chữ cái, chúng tôi xử lý các lần xuất hiện của nó theo thứ tự và đảm bảo chúng tôi không bao giờ mất nhiều hơn`k`các phần tử được chọn liên tiếp của chữ cái đó. Nếu chúng ta đạt được`k`lần xuất hiện được chọn liên tiếp, mọi lần xuất hiện tiếp theo phải được bỏ qua trừ khi chúng tôi "đặt lại" bằng cách chuyển sang một chữ cái khác, điều này xảy ra một cách tự nhiên khi chúng tôi đang xử lý theo thứ tự chung. 

Điều này dẫn đến một cấu trúc tham lam: xử lý các chỉ mục theo thứ tự, duy trì bộ đếm các ký tự được giữ liên tiếp trên mỗi khối chữ cái và bất cứ khi nào chúng ta buộc phải vượt quá`k`, chúng ta phải loại bỏ phần tử có giá trị thấp nhất trong số các lần chọn liên tiếp có cùng chữ cái hiện tại. Điều này tự nhiên trở thành một sự điều chỉnh giống như hàng đợi ưu tiên, nhưng có một cách giải thích thậm chí còn đơn giản hơn. 

Thay vì suy nghĩ linh hoạt, chúng ta có thể xử lý từng khối một cách độc lập. Trong một khối chữ cái giống hệt nhau, giả sử chiều dài của nó là`m`. Chúng tôi không thể giữ nhiều hơn`k`của chúng trong một hàng trong chuỗi cuối cùng, vì vậy trong khối đó chúng ta được phép chọn nhiều nhất`k`tổng cộng các phần tử, vì bản thân khối đã liên tiếp trong chuỗi gốc và không thể tách rời bên trong. Vì vậy, lựa chọn tối ưu trong mỗi khối là chọn`k`giá trị lớn nhất từ ​​khối đó. 

Điều này giải quyết hoàn toàn vấn đề: chúng tôi sắp xếp hoặc duy trì một lựa chọn nhỏ cho mỗi khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi bằng cách chia chuỗi thành các đoạn liền kề tối đa có các ký tự giống hệt nhau. 

1. Ta quét mảng từ trái qua phải, nhóm các ký tự bằng nhau liên tiếp thành một đoạn. Điều này là cần thiết vì chỉ trong một phân đoạn như vậy mới có thể tích lũy các máy ép liên tiếp. 
2. Đối với mỗi phân khúc, chúng tôi thu thập tất cả các giá trị thiệt hại thuộc phân khúc đó. Vì tất cả các ký tự bên trong nó đều giống nhau nên bất kỳ tập hợp con nào được chọn từ phân đoạn này sẽ xuất hiện liên tiếp trong chuỗi cuối cùng. 
3. Bên trong một phân khúc, chúng tôi muốn tối đa hóa số tiền trong khi vẫn đảm bảo rằng chúng tôi không "cam kết quá mức" vượt quá giới hạn cho phép một cách hiệu quả. Chiến lược tối ưu là chọn`min(k, length of segment)`giá trị thiệt hại lớn nhất. Lý do là bất kỳ vật phẩm nào được chọn trong phân khúc này sẽ tạo thành một khối liên tiếp nên chúng ta chỉ muốn những vật phẩm có giá trị nhất trong giới hạn cho phép. 
4. Chúng tôi sắp xếp các giá trị bên trong phân đoạn theo thứ tự giảm dần và lấy giá trị đầu tiên`k`. 
5. Chúng tôi thêm các giá trị đã chọn này vào câu trả lời và tiếp tục sang phân đoạn tiếp theo. 

Ý tưởng chính là các phân đoạn độc lập vì việc bỏ qua các phần tử không bao giờ giúp hợp nhất các ký tự khác nhau và việc hợp nhất các ký tự giống hệt nhau trên các phân đoạn khác nhau là không thể do có sự can thiệp của các chữ cái khác nhau. 

### Tại sao nó hoạt động 

Trong bất kỳ khối liền kề nào gồm các ký tự giống hệt nhau, chuỗi cuối cùng duy trì tính liền kề cho bất kỳ tập hợp con được chọn nào. Vì tất cả các phần tử trong khối đó đều đóng góp vào một lần chạy nên hạn chế có ý nghĩa duy nhất là số lượng chúng tôi giữ lại. Giữ hơn`k`từ khối đó sẽ ngay lập tức vi phạm quy tắc và việc giữ ít khối hơn luôn là điều tối ưu nếu chúng ta thay thế chúng bằng khối có giá trị cao hơn. Vì vậy, việc chọn đầu`k`các giá trị trong mỗi phân khúc tối đa hóa sự đóng góp mà không bao giờ gặp rủi ro khi chạy hợp lệ lâu hơn ở nơi khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    s = input().strip()

    ans = 0
    i = 0

    while i < n:
        j = i
        vals = []

        while j < n and s[j] == s[i]:
            vals.append(a[j])
            j += 1

        vals.sort(reverse=True)
        for t in range(min(k, len(vals))):
            ans += vals[t]

        i = j

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc dữ liệu đầu vào và lặp qua chuỗi trong khi nhóm các ký tự liên tiếp giống hệt nhau. Đối với mỗi nhóm, nó thu thập tất cả các giá trị thiệt hại tương ứng vào một danh sách. Việc sắp xếp theo thứ tự giảm dần cho phép chúng tôi chọn những lượt truy cập có giá trị nhất trước tiên và chúng tôi chỉ chọn tối đa`k`trong số họ vì lấy nhiều hơn sẽ vi phạm ràng buộc bên trong khối đó. 

Con trỏ`i`tiến lên bằng cách bỏ qua toàn bộ các phân đoạn cùng một lúc, đảm bảo việc truyền tuyến tính của chuỗi. Chi phí phi tuyến tính duy nhất đến từ việc sắp xếp trong từng phân khúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, k = 3
a = [1, 5, 16, 18, 7, 2, 10]
s = baaaaca
```Chúng tôi chia thành các phân khúc:`b | aaaa | c | a`. 

| Phân đoạn | Giá trị | Đã sắp xếp | Đã chụp | Đóng góp phân khúc | 
| --- | --- | --- | --- | --- | 
| b | [1] | [1] | [1] | 1 | 
| aaa | [5,16,18,7] | [18,16,7,5] | [18,16,7] | 41 | 
| c | [2] | [2] | [2] | 2 | 
| một | [10] | [10] | [10] | 10 | 

Tổng cộng là`1 + 41 + 2 + 10 = 54`. 

Dấu vết này cho thấy rằng quyết định hoàn toàn mang tính cục bộ đối với từng khối và các phần tử có giá trị cao hơn bên trong khối sẽ thống trị các phần tử thấp hơn bất kể vị trí. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 2
a = [10, 1, 100, 2, 3]
s = aabbb
```Phân đoạn:`aa | bbb`. 

| Phân đoạn | Giá trị | Đã sắp xếp | Đã chụp | Đóng góp | 
| --- | --- | --- | --- | --- | 
| aa | [10,1] | [10,1] | [10,1] | 11 | 
| bbb | [100,2,3] | [100,3,2] | [100,3] | 103 | 

Tổng cộng là`114`. 

Điều này chứng tỏ rằng ngay cả khi một đoạn dài hơn`k`, chỉ có điều tốt nhất`k`giá trị quan trọng và thứ tự bên trong phân khúc không ảnh hưởng đến tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phân đoạn được sắp xếp độc lập và tổng số phần tử trên các phân đoạn là n | 
| Không gian | O(n) | Chúng tôi lưu trữ các giá trị của một phân đoạn tại một thời điểm | 

Thuật toán phù hợp thoải mái trong giới hạn cho`n ≤ 2 · 10^5`, vì việc sắp xếp chiếm ưu thế nhưng được áp dụng trên dữ liệu được phân vùng thay vì trên toàn bộ dữ liệu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    s = input().strip()

    ans = 0
    i = 0

    while i < n:
        j = i
        vals = []
        while j < n and s[j] == s[i]:
            vals.append(a[j])
            j += 1

        vals.sort(reverse=True)
        for t in range(min(k, len(vals))):
            ans += vals[t]

        i = j

    return str(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("7 3\n1 5 16 18 7 2 10\nbaaaaca\n") == "54"

# all equal, k smaller than n
assert run("5 2\n5 4 3 2 1\naaaaa\n") == "9"

# k large enough
assert run("4 10\n1 2 3 4\nabcd\n") == "10"

# alternating letters
assert run("6 1\n10 9 8 7 6 5\nababab\n") == "30"

# single element
assert run("1 1\n100\nz\n") == "100"

# boundary run
assert run("6 2\n1 100 2 90 3 80\naaaaaa\n") == "190"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chữ cái giống nhau | 9 | chọn top k bên trong một khối | 
| k lớn | 10 | không có trường hợp hạn chế | 
| chữ xen kẽ | 30 | sự độc lập của các phân đoạn | 
| phần tử đơn | 100 | độ chính xác đầu vào tối thiểu | 
| khối đơn dài | 190 | lựa chọn top-k đúng | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi toàn bộ chuỗi là một ký tự. Trong trường hợp đó, thuật toán xử lý một phân đoạn đơn và chỉ cần chọn phần trên cùng`k`các giá trị. Ví dụ: 

đầu vào:```
6 2
1 100 2 90 3 80
aaaaaa
```Phân khúc thu thập`[1, 100, 2, 90, 3, 80]`, sắp xếp để`[100, 90, 80, 3, 2, 1]`, và lấy`100 + 90 = 190`. Ràng buộc hoàn toàn cục bộ nên không tồn tại tương tác với các phân đoạn khác. 

Một trường hợp cạnh khác là khi`k = 1`. Mỗi phân đoạn chỉ đóng góp phần tử tối đa của nó, phù hợp với trực giác rằng chúng ta không bao giờ có thể lấy hai chữ cái giống hệt nhau liên tiếp, vì vậy chúng ta phải giảm mọi khối thành đại diện tốt nhất của nó.
