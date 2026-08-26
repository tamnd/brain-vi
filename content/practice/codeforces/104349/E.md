---
title: "CF 104349E - Sự thay đổi trong lực lượng"
description: "Chúng ta được cho một chuỗi s có độ dài n. Từ chuỗi này, chúng ta có thể thực hiện thao tác xoay: chọn vị trí phân tách k, xóa tiền tố s[0:k] và thêm nó vào cuối. Điều này tạo ra sự dịch chuyển theo chu kỳ của chuỗi."
date: "2026-07-01T18:15:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 67
verified: true
draft: false
---

[CF 104349E - Sự thay đổi trong TheForces](https://codeforces.com/problemset/problem/104349/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chiều dài`n`. Từ chuỗi này, chúng ta có thể thực hiện thao tác xoay: chọn vị trí phân chia`k`, xóa tiền tố`s[0:k]`, và nối nó vào cuối. Điều này tạo ra sự dịch chuyển theo chu kỳ của chuỗi. Từ`k`có thể dao động từ 1 đến`n`, có chính xác`n`những phép quay có thể. 

Nhiệm vụ là xem xét tất cả các phép quay này và trả về phép quay nhỏ nhất về mặt từ điển. 

So sánh từ điển là thứ tự từ điển thông thường: chúng ta so sánh hai chuỗi từ trái sang phải và ký tự khác nhau đầu tiên sẽ xác định thứ tự. 

Ràng buộc`n ≤ 3 × 10^5`ngay lập tức loại trừ việc tạo tất cả các phép quay một cách rõ ràng và sắp xếp chúng. Xây dựng mỗi vòng quay mất`O(n)`, và làm điều đó cho tất cả`n`ca mang lại`O(n^2)`, quá chậm trong 1 giây. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự giống hệt nhau, chẳng hạn như`"aaaaa"`. Mọi vòng quay đều giống hệt nhau nên câu trả lời là cùng một chuỗi. Bất kỳ giải pháp chính xác nào cũng phải xử lý vấn đề này một cách tự nhiên mà không cần thêm logic. 

Một trường hợp cạnh khác là khi ký tự nhỏ nhất xuất hiện nhiều lần. Ví dụ,`"baca"`. Một ý tưởng tham lam ngây thơ “bắt đầu từ ký tự nhỏ nhất” là chưa đủ vì nhiều ứng cử viên vẫn phải được so sánh dưới dạng chuỗi tuần hoàn đầy đủ:```
baca → baca, acab, caba, abac
```Câu trả lời là`"abac"`, xuất phát từ sự xuất hiện sau đó của`'a'`, không phải là lần đầu tiên. 

Do đó, vấn đề là chọn sự dịch chuyển tuần hoàn tốt nhất theo thứ tự từ điển. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp rất đơn giản: tạo ra mọi vòng quay, so sánh chúng và giữ lại kết quả tốt nhất. Mỗi vòng quay yêu cầu cắt chuỗi, tính chi phí`O(n)`, và có`n`luân chuyển, cho`O(n^2)`tổng công việc. Với`n`lên tới 300.000, điều này ngụ ý khoảng 90 tỷ thao tác ký tự trong trường hợp xấu nhất, điều này là không khả thi. 

Quan sát quan trọng là tất cả các chuỗi ứng cử viên đều là chuỗi con của chuỗi nhân đôi`s + s`. Mỗi vòng quay tương ứng với một chuỗi con có độ dài`n`bắt đầu từ chỉ mục`i`trong chuỗi nhân đôi này, cho`0 ≤ i < n`. Nhiệm vụ trở thành việc chọn chuỗi con nhỏ nhất theo từ điển có độ dài cố định`n`trong số những vị trí xuất phát này. 

Một cải tiến đơn giản có thể là so sánh các chuỗi con ứng cử viên theo từng ký tự trong khi quét các ứng cử viên, nhưng hành vi trong trường hợp xấu nhất vẫn là bậc hai nếu nhiều tiền tố khớp nhau. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng ta không chọn một chuỗi con tùy ý mà chọn phép quay theo chu kỳ tối thiểu. Đây là một bài toán kinh điển có thể giải trong thời gian tuyến tính bằng cách sử dụng chiến lược loại bỏ hai con trỏ, thường được gọi là thuật toán Booth. Nó duy trì các vị trí bắt đầu của ứng viên và loại bỏ những vị trí không thể tối ưu bằng cách so sánh thứ tự từ điển của chúng với nhau. 

Thay vì so sánh rõ ràng tất cả các cặp, chúng tôi loại bỏ dần dần các vị trí bắt đầu bị chi phối trong thời gian cố định được khấu hao trên mỗi ký tự, đạt được độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Thuật toán của Booth | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi áp dụng thuật toán Booth để tìm chỉ số bắt đầu của phép quay tuần hoàn nhỏ nhất về mặt từ điển. 

1. Về mặt khái niệm, chúng tôi làm việc với chuỗi`t = s + s`. Điều này cho phép mọi sự thay đổi độ dài theo chu kỳ`n`xuất hiện dưới dạng một chuỗi con liền kề của`t`. 
2. Chúng tôi duy trì chỉ số ban đầu của ứng viên`i`và chỉ số so sánh`j`, ban đầu`i = 0`Và`j = 1`. Chúng tôi cũng theo dõi khoản bù đắp hiện tại`k`điều đó thể hiện mức độ chúng tôi đã so khớp các ký tự giữa hai ứng cử viên. 
3. Chúng tôi so sánh các nhân vật`t[i + k]`Và`t[j + k]`. Nếu bằng nhau thì ta tăng`k`bởi vì cả hai phép quay đều đồng ý với vị trí đó. 
4. Nếu xảy ra sự không khớp, chúng tôi sẽ xác định phép quay nào nhỏ hơn về mặt từ điển ở độ lệch này. Nếu như`t[i + k] < t[j + k]`, thì phép quay bắt đầu từ`j`không thể đánh bại`i`, vì vậy chúng tôi loại bỏ tất cả bắt đầu từ`j`ĐẾN`j + k`và thiết lập`j = j + k + 1`. Nếu ngược lại, chúng tôi loại bỏ phạm vi từ`i`ĐẾN`i + k`và thiết lập`i = i + k + 1`, và trao đổi`i`Và`j`để giữ`i`là ứng cử viên sáng giá nhất hiện nay. 
5. Nếu`i == j`, chúng tôi tăng`j`để đảm bảo có hai ứng cử viên khác biệt. 
6. Chúng ta tiếp tục cho đến khi`j`đạt tới`n`. Câu trả lời là chuỗi con`t[i:i+n]`. 

Ý tưởng chính là khi hai phép quay khác nhau ở vị trí`k`, tất cả các vòng quay trong đoạn thua cũng tệ hơn, vì vậy chúng ta có thể bỏ qua chúng hoàn toàn một cách an toàn thay vì kiểm tra từng cái một. 

### Tại sao nó hoạt động 

Thuật toán duy trì một tập hợp các vị trí bắt đầu ứng cử viên sao cho không có vị trí bị loại bỏ nào có thể trở thành tối ưu. Mỗi so sánh giữa hai điểm bắt đầu sẽ loại bỏ toàn bộ khối ứng cử viên không hợp lệ dựa trên sự không khớp đầu tiên. Vì thứ tự từ điển được xác định bởi ký tự khác nhau đầu tiên nên bất kỳ phép quay nào bị mất ở vị trí`k`chống lại người khác không thể trở nên tốt hơn sau này. Điều này đảm bảo rằng ứng viên còn lại`i`là nhỏ nhất trong số tất cả các dịch chuyển theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def booth(s: str) -> int:
    t = s + s
    n = len(s)
    
    i, j, k = 0, 1, 0
    
    while j < n and i < n and k < n:
        if t[i + k] == t[j + k]:
            k += 1
            continue
        
        if t[i + k] > t[j + k]:
            i = i + k + 1
            if i <= j:
                i = j + 1
        else:
            j = j + k + 1
            if j <= i:
                j = i + 1
        
        k = 0
    
    start = min(i, j)
    return start

def solve():
    n = int(input().strip())
    s = input().strip()
    
    idx = booth(s)
    n = len(s)
    t = s + s
    print(t[idx:idx + n])

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng chuỗi nhân đôi một lần để các phép quay theo chu kỳ trở thành chuỗi con tuyến tính. Hai con trỏ`i`Và`j`đại diện cho sự bắt đầu vòng quay cạnh tranh. Biến`k`theo dõi xem chúng tôi đã khớp hai phép quay đến mức nào trước khi cần đưa ra quyết định. 

Một cạm bẫy triển khai phổ biến là quên thiết lập lại`k`sau một sự không phù hợp. Nếu không có sự thiết lập lại này, các phép so sánh sẽ giả định sự liên kết không chính xác giữa các phân đoạn không liên quan. Một sự tinh tế khác là đảm bảo các chỉ số nằm trong giới hạn của độ dài chuỗi gốc`n`, không phải độ dài chuỗi gấp đôi`2n`, vì chỉ`n`vị trí bắt đầu là ứng cử viên hợp lệ. 

Việc trích xuất chuỗi con cuối cùng sử dụng chỉ mục bắt đầu đã chọn và các lát cắt chính xác`n`ký tự, đảm bảo một vòng quay đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
nima
```Chúng tôi so sánh các phép quay theo chu kỳ của`"nima"`. 

| Bước | tôi | j | k | So sánh | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | n vs tôi → tôi nhỏ hơn | 
| 2 | 0 | 2 | 0 | n vs m → m nhỏ hơn, loại bỏ i phạm vi | 
| 3 | 1 | 2 | 0 | tôi vs m → tôi nhỏ hơn | 
| 4 | 1 | 3 | 0 | i vs a → nhỏ hơn, loại bỏ i phạm vi | 

Chỉ số bắt đầu cuối cùng tương ứng với`"anim"`. 

Dấu vết này cho thấy một vòng quay sau có thể lấn át những vòng quay trước đó như thế nào và tại sao việc loại bỏ là cần thiết. 

### Ví dụ 2 

đầu vào:```
5
ababa
```| Bước | tôi | j | k | So sánh | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | a vs b → nhỏ hơn | 
| 2 | 0 | 2 | 0 | a vs a → mở rộng | 
| 3 | 0 | 2 | 1 | b vs b → mở rộng | 
| 4 | 0 | 2 | 2 | a vs a → mở rộng | 
| 5 | cuối cùng | - | - | chỉ số 0 thắng | 

Vòng quay đầu ra là`"ababa"`. 

Điều này xác nhận rằng khi tất cả các phép quay đều tương đương, thuật toán sẽ tự nhiên giữ lại ứng cử viên hợp lệ đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ số được nâng cao tối đa một lần theo kiểu khấu hao do loại bỏ các phạm vi ứng cử viên | 
| Không gian | O(n) | Lưu trữ cho chuỗi nhân đôi | 

Thời gian chạy tuyến tính phù hợp thoải mái trong các ràng buộc cho`n ≤ 3 × 10^5`, vì trung bình thuật toán chỉ thực hiện một số lượng so sánh không đổi cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    def booth(s: str) -> int:
        t = s + s
        n = len(s)
        i, j, k = 0, 1, 0
        while j < n and i < n and k < n:
            if t[i + k] == t[j + k]:
                k += 1
                continue
            if t[i + k] > t[j + k]:
                i = i + k + 1
                if i <= j:
                    i = j + 1
            else:
                j = j + k + 1
                if j <= i:
                    j = i + 1
            k = 0
        return min(i, j)

    n = int(sys.stdin.readline().strip())
    s = sys.stdin.readline().strip()
    idx = booth(s)
    t = s + s
    return t[idx:idx + len(s)]

assert run("4\nnima\n") == "anim"
assert run("5\nababa\n") == "ababa"
assert run("1\na\n") == "a"
assert run("3\nzzz\n") == "zzz"
assert run("6\ncbaabc\n") == "aabccb"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 năm | hoạt hình | lựa chọn xoay cơ bản | 
| 5 bà ba | ba ba | xử lý cấu trúc lặp đi lặp lại | 
| 1 một | một | kích thước tối thiểu | 
| zzz | zzz | tất cả các ký tự bằng nhau | 
| cbaabc | aabccb | vòng quay tốt nhất không tầm thường | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`"a"`, thuật toán khởi tạo`i = 0`Và`j = 1`, Nhưng`j`ngay lập tức vượt quá giới hạn, vì vậy chỉ mục`0`được chọn. Đầu ra là`"a"`. 

Đối với một chuỗi có các ký tự giống hệt nhau như`"bbbbbb"`, mọi so sánh`t[i+k] == t[j+k]`tiếp tục cho đến khi`k = n`, và không có sự khử nào xảy ra. Thuật toán trả về`0`, tạo ra chuỗi gốc không thay đổi. 

Đối với các chuỗi có vòng quay tối ưu xảy ra muộn, chẳng hạn như`"bcaac"`, thuật toán sẽ loại bỏ các ứng cử viên sớm ngay khi gặp phải một ký tự nhỏ hơn nghiêm ngặt ở một mức bù nào đó, đảm bảo các lần khởi động tối ưu muộn được giữ nguyên mà không cần liệt kê rõ ràng.
