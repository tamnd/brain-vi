---
title: "CF 1054B - Bổ sung Mex"
description: "Chúng ta được cung cấp một chuỗi được cho là được xây dựng từng bước bắt đầu từ một mảng trống. Ở mỗi bước, trình xây dựng được phép xem xét bất kỳ tập hợp con nào của các phần tử đã tồn tại trong mảng, tính toán mex của tập hợp con đó và nối mex đó làm phần tử tiếp theo."
date: "2026-06-15T10:22:07+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1054
codeforces_index: "B"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 1"
rating: 1000
weight: 1054
solve_time_s: 152
verified: true
draft: false
---

[CF 1054B - Bổ sung Mex](https://codeforces.com/problemset/problem/1054/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi được cho là được xây dựng từng bước bắt đầu từ một mảng trống. Ở mỗi bước, trình xây dựng được phép xem xét bất kỳ tập hợp con nào của các phần tử đã tồn tại trong mảng, tính toán mex của tập hợp con đó và nối mex đó làm phần tử tiếp theo. 

Mex của nhiều tập hợp là số nguyên không âm nhỏ nhất không xuất hiện trong đó. Vì vậy, quy trình này cực kỳ linh hoạt: ở mỗi bước, bạn có thể “truy vấn” bất kỳ tập hợp con nào trong quá khứ và nối thêm giá trị còn thiếu. 

Nhiệm vụ không phải là xây dựng lại bản thân công trình mà là phát hiện tính nhất quán. Chúng ta được cung cấp mảng cuối cùng và chúng ta phải quyết định xem có tồn tại chuỗi các lựa chọn tập hợp con nào có thể tạo ra mảng đó hay không. Nếu có, chúng tôi xuất -1. Nếu không, chúng ta phải xuất tiền tố sớm nhất trong đó một số bước không thể thực hiện được trong bất kỳ cấu trúc hợp lệ nào. 

Ràng buộc n lên tới 100000 buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì cố gắng tính toán lại mex trên nhiều tập hợp con hoặc mô phỏng tất cả các lựa chọn đều không thể thực hiện được ngay lập tức vì số lượng tập hợp con tăng theo cấp số nhân. Ngay cả việc duy trì cấu trúc động trên mỗi bước cũng phải là O(log n) hoặc cao hơn. 

Điểm tinh tế quan trọng là phép toán chỉ phụ thuộc vào tập hợp các giá trị có trong tập hợp con đã chọn, chứ không phụ thuộc vào thứ tự hoặc bội số của chúng ngoài sự hiện diện. Điều này có nghĩa là lý luận về cơ bản là về những giá trị nào đã có sẵn trên toàn cầu ở mỗi bước. 

Một vài trường hợp thất bại rất dễ bỏ sót: 

Nếu phần tử đầu tiên khác 0 thì việc xây dựng ngay lập tức là không thể. Vì mảng ban đầu trống nên tập hợp con duy nhất trống, có mex bằng 0, nên phần tử đầu tiên phải luôn bằng 0. 

Một tình huống phức tạp khác phát sinh khi xuất hiện một số không thể tạo được dựa trên “bộ giá trị có sẵn” hiện tại. Ví dụ: nếu chúng ta thấy một giá trị x nhưng chúng ta chưa đảm bảo rằng tất cả các giá trị từ 0 đến x−1 đã xuất hiện ở đâu đó trước đó trong tiền tố thì x không thể được tạo ra dưới dạng mex của bất kỳ tập hợp con nào. 

Cuối cùng, khi chúng ta nhìn thấy một số, nó sẽ ảnh hưởng đến tính khả thi trong tương lai vì nó có thể sử dụng được trong các tập hợp con. Vì vậy, chúng ta phải duy trì một bộ “số sẵn có” ngày càng tăng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp sẽ cố gắng mô phỏng từng bước: đối với mỗi vị trí m, hãy xem xét tất cả các tập hợp con của các phần tử trước đó, tính mex cho từng tập hợp con và kiểm tra xem giá trị hiện tại có thể đạt được hay không. Về nguyên tắc, điều này đúng vì nó phản ánh chính xác định nghĩa. Tuy nhiên, ngay cả đối với một bước cũng có 2^(m−1) tập hợp con, khiến cho số mũ này trở thành cấp số nhân. Với n = 100000, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần liệt kê các tập hợp con. Mex của một tập hợp con chỉ phụ thuộc vào giá trị nào từ 0 trở lên có trong tập hợp con đó. Nếu một tập hợp con có thể tạo ra mex x thì tập hợp con đó phải chứa mọi số nguyên từ 0 đến x−1 và phải thiếu x. Vì vậy, câu hỏi thực sự trở thành: ở bước i, có thể tồn tại một số tập hợp con của các phần tử đã thấy trước đó có nội dung cho phép mex = a[i] không? 

Điều này dẫn đến điều kiện cấu trúc trên tiền tố toàn cục: chúng ta phải có khả năng đảm bảo rằng tất cả các số nhỏ hơn a[i] tồn tại ở đâu đó trong các phần tử trước đó, nếu không thì không tập hợp con nào có thể bao gồm chúng cùng một lúc. Ngoài ra, nếu a[i] lớn hơn giá trị hiện tại “có thể xây dựng được” thì điều đó có thể gây ra mâu thuẫn. 

Một cách rõ ràng để theo dõi tính khả thi là duy trì những giá trị đã xuất hiện trong tiền tố. Sau đó, chúng ta có thể kiểm tra xem giá trị hiện tại có phù hợp với kết quả mex hay không. Việc xây dựng không thành công chính xác khi chúng tôi được yêu cầu tạo ra một giá trị không phải là mex của bất kỳ tập hợp con nào dựa trên cấu trúc tập hợp có sẵn, điều này làm giảm việc kiểm tra xem tất cả các giá trị nhỏ hơn có tồn tại trong tiền tố khi cần hay không và liệu các ràng buộc thứ tự có bị vi phạm hay không.

Giải pháp cuối cùng trở thành một bước duy nhất duy trì một bộ tần số và theo dõi ngầm mẫu giá trị bị thiếu nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mảng từ trái sang phải trong khi vẫn duy trì những giá trị đã xuất hiện. 

1. Chúng ta khởi tạo một tập hợp hoặc mảng boolean`seen`để ghi lại những số nguyên nào đã xuất hiện cho đến nay và một con trỏ`mex`bắt đầu từ 0. 

Con trỏ biểu thị số nguyên không âm nhỏ nhất chưa thấy trong tiền tố. 
2. Với mỗi vị trí i từ 1 đến n, ta đọc a[i]. 
3. Nếu a[i] lớn hơn mex, chúng ta kết luận ngay rằng việc xây dựng là không thể. 

Lý do là mex của bất kỳ tập hợp con nào của các phần tử trước đó không thể vượt quá mex toàn cục của tiền tố, bởi vì bất kỳ tập hợp con nào cũng chỉ có thể sử dụng các giá trị đã có trong tiền tố. 
4. Nếu a[i] bằng mex, điều này luôn có thể đạt được: chúng ta có thể lấy một tập hợp con chứa tất cả các giá trị từ 0 đến mex−1 (phải tồn tại trong tiền tố) và bỏ qua chính mex, tạo ra kết quả là mex. Vì vậy chúng tôi chấp nhận bước này. 
5. Nếu a[i] nhỏ hơn mex thì điều đó cũng có thể đạt được vì chúng ta có thể chọn một tập hợp con chứa tất cả các số từ 0 đến a[i]−1 nhưng loại trừ a[i], điều này có thể xảy ra vì mex lớn hơn, nghĩa là tất cả các giá trị đó đều tồn tại. 
6. Sau khi xử lý a[i], chúng tôi đánh dấu nó là đã xem và nâng cao mex trong khi đã thấy [mex] là đúng. 

Chúng tôi theo dõi vị trí sớm nhất nơi kích hoạt bước 3. Chỉ số đó là sai lầm được đảm bảo đầu tiên. 

### Tại sao nó hoạt động 

Tính bất biến đó là`mex`luôn đại diện cho số nhỏ nhất không có ở bất kỳ đâu trong tiền tố. Bất kỳ tập hợp con mex nào cũng phải nằm trong khoảng từ 0 đến mex. Nếu một giá trị lớn hơn mex xuất hiện ở đầu ra, điều đó có nghĩa là một tập hợp con tạo ra mex lớn hơn giá trị mà tiền tố cho phép, điều này mâu thuẫn với định nghĩa của mex. Ngược lại, bất kỳ giá trị nào nhỏ hơn hoặc bằng mex đều có thể đạt được vì tiền tố đã chứa tất cả các số nhỏ hơn cần thiết, cho phép xây dựng một tập hợp con có mex khớp với mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    seen = set()
    mex = 0

    for i, x in enumerate(a):
        if x > mex:
            print(i + 1)
            return

        seen.add(x)
        while mex in seen:
            mex += 1

    print(-1)

if __name__ == "__main__":
    solve()
```Mã này duy trì một tập hợp các giá trị đang chạy đã thấy trong mảng và liên tục cập nhật mex toàn cầu hiện tại. Việc kiểm tra khóa diễn ra trước khi chèn giá trị hiện tại: nếu giá trị vượt quá mex hiện tại, thì nó vi phạm giới hạn cấu trúc mà không tập hợp con nào có thể tạo ra mex lớn hơn giá trị bị thiếu trên toàn cầu. 

Bước cập nhật đảm bảo mex luôn phản ánh trạng thái tiền tố và điều này rất cần thiết vì hiệu lực trong tương lai phụ thuộc vào giá trị nào đã được đưa vào cho đến nay. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 1 2 1
```Chúng tôi theo dõi`seen`Và`mex`: 

| tôi | một [tôi] | đã thấy trước đây | mex trước | hành động | mex sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | {} | 0 | thêm 0 | 1 | 
| 2 | 1 | {0} | 1 | thêm 1 | 2 | 
| 3 | 2 | {0,1} | 2 | thêm 2 | 3 | 
| 4 | 1 | {0,1,2} | 3 | thêm 1 | 3 | 

Không có lúc nào giá trị vượt quá mex, do đó trình tự nhất quán. Thuật toán đưa ra -1, phù hợp với thực tế là việc xây dựng là khả thi. 

### Ví dụ 2 

đầu vào:```
3
0 2 1
```| tôi | một [tôi] | đã thấy trước đây | mex trước | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | {} | 0 | được rồi, mex trở thành 1 | 
| 2 | 2 | {0} | 1 | 2 > 1, thất bại | 

Ở bước 2, chúng ta gặp 2 trong khi mex là 1. Điều này là không thể vì không có tập con nào của {0} có thể tạo ra mex 2. Thuật toán cho ra 2. 

Điều này khẳng định rằng vi phạm đầu tiên được phát hiện ngay lập tức khi giới hạn cấu trúc bị phá vỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được chèn một lần và tổng thể mex tăng lên tối đa n lần | 
| Không gian | O(n) | Bộ lưu trữ tối đa n giá trị riêng biệt | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó, một đường tuyến tính duy nhất với các cập nhật liên tục hoặc khấu hao không đổi phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""

# We redefine properly for testing
def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    seen = set()
    mex = 0

    for i, x in enumerate(a):
        if x > mex:
            return str(i + 1)
        seen.add(x)
        while mex in seen:
            mex += 1

    return "-1"

def run(inp: str) -> str:
    return solve_io(inp)

# provided samples
assert run("4\n0 1 2 1\n") == "-1", "sample 1"

# custom tests
assert run("1\n0\n") == "-1", "single valid"
assert run("1\n1\n") == "1", "must start with 0"
assert run("3\n0 2 1\n") == "2", "early violation"
assert run("5\n0 1 2 3 0\n") == "-1", "restarts allowed"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử hợp lệ | -1 | xây dựng hợp lệ tối thiểu | 
| phần tử đầu tiên 1 | 1 | cưỡng bức thất bại ở bước 1 | 
| 0 2 1 | 2 | vi phạm mex sớm | 
| 0 1 2 3 0 | -1 | tăng lớn rồi đặt lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi phần tử đầu tiên khác 0. Đối với đầu vào`1`, tập con duy nhất ở bước 1 trống, có mex bằng 0, do đó, bất kỳ giá trị nào khác 0 ngay lập tức ngụ ý không thể thực hiện được. Thuật toán xử lý việc này vì`x > mex`đúng ở bước đầu tiên khi mex vẫn bằng 0. 

Một trường hợp khác là khi các giá trị nhảy lên trên mex hiện tại, chẳng hạn như`0 5 ...`. Ở bước 2, mex là 1, vì vậy nhìn thấy 5 sẽ gây ra lỗi ngay lập tức. Điều này nắm bắt chính xác thực tế là không có tập hợp con của`{0}`có thể tạo ra mex lớn hơn 1. 

Một trường hợp tinh vi được lặp lại các giá trị nhỏ như`0 0 0 0`. Ở đây mex giữ nguyên 1 sau lần chèn đầu tiên và tất cả các số 0 tiếp theo đều hợp lệ vì các tập hợp con luôn có thể tạo ra 0 bằng cách chọn một tập hợp con trống hoặc một tập hợp con bị thiếu 0. Thuật toán không bao giờ gây ra lỗi một cách chính xác vì không có giá trị nào vượt quá mex.
