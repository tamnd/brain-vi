---
title: "CF 105364A - Cặp"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi số, chúng ta nhận được một danh sách các số nguyên có độ dài chẵn và chúng ta phải quyết định xem có thể chia các số thành từng cặp sao cho mỗi cặp có tổng bằng nhau hay không."
date: "2026-06-23T16:00:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105364
codeforces_index: "A"
codeforces_contest_name: "XXV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105364
solve_time_s: 75
verified: true
draft: false
---

[CF 105364A - Cặp](https://codeforces.com/problemset/problem/105364/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi số, chúng ta nhận được một danh sách các số nguyên có độ dài chẵn và chúng ta phải quyết định xem có thể chia các số thành từng cặp sao cho mỗi cặp có tổng bằng nhau hay không. Mỗi phần tử phải được sử dụng chính xác một lần và tất cả các cặp được hình thành phải có chung một tổng mục tiêu duy nhất. 

Một cách khác để suy nghĩ về nhiệm vụ là chúng ta đang cố gắng sắp xếp lại mảng thành các cặp rời rạc và mỗi cặp phải "cân bằng" về cùng một giá trị. Thách thức không phải là xây dựng các cặp một cách rõ ràng mà chỉ là xác định xem liệu một cặp như vậy có tồn tại hay không. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào cố gắng kiểm tra các cặp một cách rõ ràng sẽ thất bại. Với tối đa$5 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, một giải pháp xem xét ngay cả số cặp bậc hai cho mỗi trường hợp là không thể ngay lập tức. Điều này buộc chúng ta phải hướng tới một chiến lược trong đó mỗi trường hợp kiểm thử được xử lý theo thời gian tuyến tính hoặc gần tuyến tính, điển hình là$O(n \log n)$hoặc tốt hơn. 

Một vấn đề nhỏ xuất hiện khi tất cả các giá trị giống hệt nhau hoặc khi các giá trị lặp lại với sự phân bố không đồng đều. Ví dụ: nếu tất cả các số đều giống nhau thì luôn có thể ghép đôi vì mọi cặp đều có tổng bằng nhau. Mặt khác, nếu nhiều tập hợp bị lệch sao cho chỉ một tổng ứng cử viên có thể hoạt động cho một cặp nhưng không nhất quán cho tất cả các cặp, thì việc ghép nối tham lam ngây thơ có thể giả định không chính xác tính khả thi hoặc thất bại tùy thuộc vào thứ tự ghép nối. 

Hãy xem xét một trường hợp gây hiểu lầm như:```
4
1 2 3 4
```Nếu chúng ta ghép các phần tử liền kề một cách tham lam, chúng ta sẽ nhận được tổng 3 và 7, kết quả này ngay lập tức thất bại. Nhưng ngay cả những cặp thay thế như (1,4) và (2,3) cũng cho tổng 5 và 5, vì vậy trường hợp này thực sự hợp lệ. Điều này cho thấy rằng thứ tự ghép nối có ý nghĩa quan trọng đối với các chiến lược đơn giản và chúng ta cần một bất biến toàn cục thay vì các quyết định cục bộ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng hình thành tất cả các cặp có thể có của mảng và kiểm tra xem liệu bất kỳ cặp đầy đủ nào có tạo ra tổng bằng nhau trên tất cả các cặp hay không. Điều này tương đương với việc khám phá tất cả các kết quả khớp hoàn hảo trong một biểu đồ hoàn chỉnh với$n$các đỉnh tăng theo cấp số nhân. Ngay cả khi cắt tỉa, số lượng các cặp vẫn theo thứ tự$(n-1)!!$, điều này trở nên không khả thi ngay cả đối với$n = 20$. 

Quan sát quan trọng là nếu tồn tại một cặp như vậy thì tổng mục tiêu phải được xác định duy nhất. Giả sử chúng ta sắp xếp mảng. Nếu chúng ta ghép phần tử nhỏ nhất với phần tử lớn nhất thì phần tử đó đã cố định số tiền cần thiết. Mọi cặp khác phải tuân theo cùng một tổng này, điều này buộc phải có một cấu trúc chặt chẽ: cặp nhỏ thứ hai với cặp lớn thứ hai, v.v. Nếu xảy ra bất kỳ sự không khớp nào, thì không có sự ghép nối thay thế nào có thể giải quyết được tình huống này, bởi vì việc thay đổi bất kỳ sự ghép nối nào sẽ phá vỡ tính nhất quán của tổng toàn cầu. 

Điều này làm giảm vấn đề từ việc tìm kiếm trên tất cả các cặp đến việc xác minh một mẫu ghép nối xác định duy nhất sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(1) / O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc mảng và sắp xếp theo thứ tự không giảm. Việc sắp xếp là cần thiết vì cấu trúc ghép đôi tối ưu phụ thuộc vào việc kết hợp các cực trị và việc sắp xếp sẽ hiển thị rõ ràng các cực trị này. 
2. Tính tổng mục tiêu ứng cử viên bằng cách sử dụng phần tử đầu tiên và cuối cùng, cụ thể$a[0] + a[n-1]$. Giá trị này là bắt buộc nếu tồn tại một giải pháp hợp lệ, vì các phần tử nhỏ nhất và lớn nhất phải được ghép nối trong bất kỳ cấu hình hợp lệ nào; nếu không, việc thay thế chúng bằng bất kỳ cặp nào khác sẽ chỉ làm giảm phạm vi và phá vỡ tính nhất quán. 
3. Sử dụng hai con trỏ, một con trỏ bắt đầu ở đầu và một con trỏ ở cuối rồi di chuyển vào trong. Ở mỗi bước, tạo thành một cặp khái niệm từ phần tử nhỏ nhất còn lại và phần tử lớn nhất còn lại hiện tại. 
4. Đối với mỗi cặp như vậy, hãy xác minh rằng tổng của chúng bằng tổng mục tiêu đã tính toán trước đó. Nếu bất kỳ cặp nào bị sai lệch, hãy kết luận ngay rằng không có cặp nào hợp lệ tồn tại. 
5. Nếu tất cả các cặp khớp với tổng mục tiêu thì tồn tại một phân vùng hợp lệ. 

Ý tưởng chính là khi các phần tử ngoài cùng được cố định, cấu trúc còn lại sẽ được xác định đầy đủ. Không còn sự linh hoạt để sắp xếp lại mà không phá vỡ tính nhất quán. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, giả sử tồn tại một giải pháp hợp lệ. Để phần tử tối thiểu được ghép với một số phần tử khác với phần tử tối đa. Nếu điều đó xảy ra, giá trị lớn nhất phải ghép với giá trị nhỏ hơn hoặc bằng giá trị lớn thứ hai. Điều đó sẽ buộc một cặp thứ hai có tổng vượt quá hoặc thấp hơn mục tiêu ban đầu tùy theo thứ tự, mâu thuẫn với yêu cầu tất cả các cặp có cùng một tổng. 

Điều này tạo ra một cấu trúc ghép nối cứng nhắc: mảng được sắp xếp phải ghép đôi đối xứng từ đầu vào trong. Vì tổng mục tiêu được cố định bởi các phần tử cực trị nên mọi cặp đều bị ép buộc và bất kỳ sai lệch nào cũng sẽ làm mất hiệu lực cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        target = a[0] + a[-1]

        ok = True
        i, j = 0, n - 1

        while i < j:
            if a[i] + a[j] != target:
                ok = False
                break
            i += 1
            j -= 1

        out.append("SI" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho giá trị nhỏ nhất và lớn nhất trở thành cặp ứng cử viên đầu tiên tự nhiên. Tổng mục tiêu được cố định ngay lập tức từ các cực trị này, vì bất kỳ giải pháp hợp lệ nào cũng phải bao gồm một số cấu trúc ghép nối phù hợp với giá trị đó. 

Vòng lặp hai con trỏ thực thi rằng mọi cặp đối xứng đều khớp với cùng một tổng. Vòng lặp chỉ chạy$n/2$lặp đi lặp lại, do đó bước xác minh là tuyến tính. Yêu cầu tinh tế duy nhất là mục tiêu phải được tính toán trước khi vòng lặp bắt đầu; việc tính toán lại nó một cách linh hoạt sẽ không chính xác vì nó sẽ cho phép các điều chỉnh cục bộ không nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
-2 1 -1 2
```Mảng được sắp xếp:`[-2, -1, 1, 2]`, mục tiêu = 0 

| tôi | j | một [tôi] | một[j] | tổng hợp | hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | -2 | 2 | 0 | vâng | 
| 1 | 2 | -1 | 1 | 0 | vâng | 

Tất cả các cặp đều khớp nhau, vì vậy đầu ra là`SI`. 

Điều này xác nhận rằng việc ghép nối đối xứng nắm bắt chính xác cấu trúc hợp lệ ngay cả khi có âm bản. 

### Ví dụ 2 

đầu vào:```
6
2 4 0 6 3 5
```Mảng được sắp xếp:`[0, 2, 3, 4, 5, 6]`, mục tiêu = 6 

| tôi | j | một [tôi] | một[j] | tổng hợp | hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 6 | 6 | vâng | 
| 1 | 4 | 2 | 5 | 7 | không | 

Sự không khớp xảy ra ngay lập tức, do đó đầu ra là`NO`. 

Điều này cho thấy rằng mặc dù một số cặp có thể khớp với mục tiêu riêng lẻ nhưng cấu trúc toàn cầu không thể được thỏa mãn đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm | chỉ các con trỏ và các biến cố định, ngoài bộ nhớ đầu vào | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi$5 \cdot 10^5$, do đó việc sắp xếp từng trường hợp một cách độc lập vẫn phù hợp thoải mái trong giới hạn. Quá trình quét tuyến tính sau đó đảm bảo rằng việc xác minh không làm tăng thêm chi phí ngoài việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        target = a[0] + a[-1]
        ok = True
        i, j = 0, n - 1
        while i < j:
            if a[i] + a[j] != target:
                ok = False
                break
            i += 1
            j -= 1
        res.append("SI" if ok else "NO")
    return "\n".join(res)

# provided samples
assert run("""3
4
-2 1 -1 2
6
2 4 0 6 3 5
8
1 1 1 1 1 1 1 1
""") == """SI
NO
SI"""

# all equal values
assert run("""1
6
5 5 5 5 5 5
""") == "SI"

# already valid symmetric structure
assert run("""1
4
1 3 2 2
""") == "SI"

# impossible due to imbalance
assert run("""1
4
1 1 2 3
""") == "NO"

# negative values
assert run("""1
4
-1 -2 2 1
""") == "SI"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | SI | đồng phục multiset luôn hoạt động | 
| ghép đôi ẩn đối xứng | SI | tính đúng đắn vượt quá trật tự tầm thường | 
| trường hợp mất cân bằng | KHÔNG | phát hiện các phân phối không thể | 
| âm bản | SI | tính đúng đắn khi đổi dấu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều giống hệt nhau. Đối với đầu vào:```
6
7 7 7 7 7 7
```việc sắp xếp cho cùng một mảng và mục tiêu là 14. Mỗi cặp có tổng bằng 14, do đó thuật toán chấp nhận. Quá trình quét con trỏ không bao giờ tìm thấy sự không khớp, xác nhận tính chính xác trong phân bố đồng nhất suy biến. 

Một trường hợp khác liên quan đến số âm trong đó việc ghép nối tối ưu không rõ ràng nếu không sắp xếp:```
4
-3 -1 2 4
```Sắp xếp mang lại`[-3, -1, 2, 4]`, target = 1. Các cặp là (-3,4) và (-1,2), cả hai đều có tổng bằng 1. Thuật toán xác định chính xác tính khả thi ngay cả khi việc ghép đôi liền kề ngây thơ sẽ gợi ý không chính xác sự thất bại. 

Trường hợp cạnh thứ ba là khi chỉ có một cặp vi phạm ràng buộc:```
6
0 1 2 3 4 10
```Mảng được sắp xếp dẫn đến mục tiêu 10, nhưng việc ghép nối sớm cho thấy sự không khớp. Thuật toán sẽ loại bỏ ngay lập tức mà không cần khám phá các cặp thay thế, điều này cần thiết để mang lại hiệu quả cho các đầu vào lớn.
