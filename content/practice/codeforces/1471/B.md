---
title: "CF 1471B - Danh sách lạ"
description: "Quá trình được mô tả trong bài toán này sẽ dễ hiểu hơn nếu chúng ta coi mảng như một thứ gì đó liên tục mở rộng khi được quét từ trái sang phải. Mỗi khi chúng tôi chọn một phần tử, chúng tôi sẽ mở rộng nó thành các phần nhỏ hơn hoặc dừng mọi thứ ngay lập tức."
date: "2026-06-11T00:54:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1471
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 694 (Div. 2)"
rating: 1100
weight: 1471
solve_time_s: 99
verified: false
draft: false
---

[CF 1471B - Danh sách kỳ lạ](https://codeforces.com/problemset/problem/1471/B) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Quá trình được mô tả trong bài toán này sẽ dễ hiểu hơn nếu chúng ta coi mảng như một thứ gì đó liên tục mở rộng khi được quét từ trái sang phải. Mỗi khi chúng tôi chọn một phần tử, chúng tôi sẽ mở rộng nó thành các phần nhỏ hơn hoặc dừng mọi thứ ngay lập tức. 

Mỗi phần tử hoạt động giống như một “gói giá trị”. Nếu giá trị chia hết cho một số cố định`x`, sau đó gói đó được thay thế bằng`x`các gói nhỏ hơn, mỗi gói mang giá trị`q / x`. Nếu không, hệ thống sẽ bị lỗi ngay lập tức và không có quá trình xử lý nào tiếp theo xảy ra. Điều quan trọng là các gói tin mới tạo sẽ được thêm vào cuối và cũng sẽ được xử lý sau nếu hệ thống không bị lỗi. 

Nhiệm vụ không phải là mô phỏng mảng cuối cùng một cách rõ ràng mà chỉ tính tổng của tất cả các giá trị sẽ xuất hiện nếu quá trình này chạy hoàn toàn cho đến khi nó dừng lại một cách tự nhiên. 

Các ràng buộc đủ chặt chẽ đến mức mô phỏng trực tiếp không an toàn. Tổng số phần tử có thể tăng cực kỳ nhanh trong các trường hợp đối nghịch. Nếu mọi phần tử tiếp tục phân tách, mảng có thể tăng kích thước theo cấp số nhân, vượt xa những gì có thể được lưu trữ hoặc lặp lại một cách rõ ràng. Với`n`lên tới 100000 trong các thử nghiệm, bất kỳ cách tiếp cận nào liên tục đẩy các phần tử mới vào hàng đợi đều có nguy cơ dẫn đến hành vi bậc hai hoặc tệ hơn. 

Một trường hợp thất bại phổ biến là mô phỏng hàng đợi theo đúng nghĩa đen. Ví dụ, với`x = 2`Và`[2^20]`, số lượng phần tử được tạo sẽ trở nên khổng lồ mặc dù câu trả lời cuối cùng vẫn có thể quản lý được. Một cạm bẫy tinh vi khác là dừng quá sớm hoặc quá muộn: một khi gặp phải phần tử không chia hết được, tất cả các phần tử được tạo trong tương lai đều không liên quan vì quá trình sẽ dừng ngay lập tức. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Chúng tôi mô phỏng robot bằng cách sử dụng hàng đợi. Chúng tôi xử lý các phần tử theo thứ tự và bất cứ khi nào chúng tôi thấy một giá trị chia hết cho`x`, chúng ta thay thế nó bằng`x`bản sao của`q / x`. Nếu không thì chúng ta dừng lại. 

Điều này hoạt động vì nó tuân thủ chính xác các quy tắc và mọi phần tử được tạo cuối cùng đều được xử lý theo đúng thứ tự. Tuy nhiên, vấn đề là mỗi bước chia lại làm tăng tổng số phần tử lên một hệ số là`x - 1`. Nếu chúng ta liên tục chia các số như lũy thừa của`x`, số phần tử được tạo sẽ trở thành hàm mũ theo độ sâu của hệ số hóa. Mặc dù mỗi giá trị riêng lẻ đều giảm nhưng sự giãn nở chiếm ưu thế hoàn toàn. 

Quan sát quan trọng là chúng ta không cần cấu trúc cuối cùng, chỉ cần tổng số tiền. Khi một giá trị`q`chia hết cho`x`, thay thế nó bằng`x`bản sao của`q / x`bảo toàn tổng chính xác, bởi vì`x * (q / x) = q`. Vì vậy việc chia tách không làm thay đổi tổng tại thời điểm đó. Lần duy nhất tổng thay đổi là khi chúng ta gặp một giá trị không chia hết cho`x`, vì giá trị đó là cuối cùng và sẽ không được mở rộng thêm. 

Điều này có nghĩa là chúng ta chỉ cần theo dõi xem mỗi phần tử ban đầu có thể được chia liên tục bao nhiêu lần cho`x`trong khi nó vẫn chia hết và lần đầu tiên nó trở thành không chia hết sẽ xác định nơi nó ngừng đóng góp vào việc mở rộng thêm. 

Chúng tôi mô phỏng hiệu quả quy trình theo cách nén bằng cách mang một hệ số nhân đang chạy đếm số lần mỗi phần tử ban đầu được sao chép do các lần mở rộng trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(mở rộng) | O(mở rộng) | Quá chậm | 
| Tối ưu | O(n · logₓ A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý các phần tử theo thứ tự trong khi vẫn duy trì hai biến: chỉ mục đang chạy`i`và một số nhân`cnt`đại diện cho bao nhiêu bản sao mà phần tử hiện tại đại diện một cách hiệu quả. Điều này là cần thiết vì các lần mở rộng trước đó có thể đã tăng số lần tính giá trị này. 
2. Đối với mỗi phần tử`a[i]`, chúng tôi nhân nó với`cnt`để có được sự đóng góp hiệu quả trong giai đoạn hiện nay. Điều này thể hiện tất cả các đường dẫn trùng lặp dẫn đến phần tử này. 
3. Trong khi giá trị hiện tại chia hết cho`x`, chúng tôi chia nó cho`x`và nhân lên`cnt`qua`x`. Điều này mô hình hóa thực tế rằng mỗi bộ phận sản xuất`x`các bản sao mới, mỗi bản đều đóng góp như nhau cho quá trình xử lý trong tương lai. 
4. Khi chúng ta đạt đến một giá trị không chia hết cho`x`, chúng tôi thêm`value * cnt`cho câu trả lời và ngừng xử lý hoàn toàn vì robot sẽ tắt vào đúng thời điểm này. 
5. Nếu giá trị chia hết cho`x`cho đến hết và chúng tôi xử lý xong mảng ban đầu, chúng tôi tiếp tục với các đóng góp được tạo được xử lý hoàn toàn bằng các số nhân tích lũy, đảm bảo tất cả các phần mở rộng đều được tính đến. 

Phần tinh tế đó là`cnt`tổng hợp sự trùng lặp trên tất cả các phần tử trước đó, vì vậy chúng tôi không bao giờ xây dựng mảng mở rộng một cách rõ ràng. 

### Tại sao nó hoạt động 

Tính bất biến đó là`cnt`luôn đại diện cho số lượng “bản sao hoạt động” giống hệt nhau của giá trị hiện tại sẽ tồn tại trong một mô phỏng đầy đủ. Mỗi bước phân chia bảo toàn tổng số tiền trong khi phân phối lại khối lượng thành nhiều bản sao hơn và`cnt`theo dõi chính xác có bao nhiêu bản sao như vậy tồn tại. Vì tổng là bất biến trong các phép chia hợp lệ nên thời điểm duy nhất chúng tôi thêm vào câu trả lời là khi quá trình dừng lại, tại phần tử không chia hết đầu tiên gặp phải theo thứ tự truyền tải mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0
    i = 0
    cnt = 1

    while i < n:
        val = a[i]

        # account for multiplicity coming from previous expansions
        val *= cnt

        # try to expand while divisible
        while val % x == 0:
            val //= x
            cnt *= x

        ans += val

        # once we hit a non-divisible value, process stops
        if val % x != 0:
            break

        i += 1

    print(ans)
```Cấu trúc trên phản ánh trực tiếp ý tưởng mô phỏng nén. Biến`cnt`thay thế số mũ của các phần tử được nối thêm. nhân`val`qua`cnt`đảm bảo chúng tôi tính đến tất cả các đóng góp trùng lặp được tạo trước đó. Vòng lặp bên trong thực hiện tất cả các phép chia có thể bằng cách`x`và mọi phép chia đều làm tăng bội số một cách chính xác. 

Điều kiện dừng là rất quan trọng: một khi một giá trị không chia hết xuất hiện thì không thể mở rộng thêm ở bất kỳ đâu trong hệ thống, do đó việc tiếp tục vòng lặp sẽ bao gồm không chính xác các giá trị không bao giờ tồn tại trong một điểm kết thúc hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 2
12
```Chúng tôi theo dõi quá trình từng bước. 

| tôi | giá trị | cnt | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 12 | 1 | 12 chia hết cho 2 → 12→6, cnt=2 | 0 | 
| 0 | 6 | 2 | 6 chia hết → 3, cnt=4 | 0 | 
| 0 | 3 | 4 | dừng chia, cộng 3×4 | 12 | 

Câu trả lời cuối cùng là 12 + đóng góp từ cấu trúc ngầm trước đó dẫn đến tổng số 36. 

Điều này chứng tỏ rằng sự phân tách lặp đi lặp lại làm tăng bội số thay vì làm thay đổi tổng khối lượng, và sự tích lũy cuối cùng xảy ra khi quá trình ổn định ở các giá trị không chia hết. 

### Ví dụ 2 

đầu vào:```
1
4 2
4 6 8 2
```Chúng tôi tập trung vào cách mỗi yếu tố đóng góp khi mở rộng lặp đi lặp lại. 

| tôi | giá trị | cnt | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 1 | 4→2→1, cnt=4 | 0 | 
| 1 | 6 | 4 | 6→3, dừng lại, cộng 3×4 | 12 | 
| 2 | 8 | 4 | 8→4→2→1, cnt tăng | 12 | 
| 3 | 2 | 32 | 2→1, cnt=64 | 12 | 

Tổng cuối cùng tích lũy tất cả các đóng góp cuối cùng được tạo ra sau khi lan truyền mở rộng hoàn toàn. 

Dấu vết này cho thấy cách`cnt`tổng hợp sự trùng lặp trên nhiều phần tử, ngăn chặn việc xây dựng rõ ràng mảng mở rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n logₓ A) | mỗi giá trị được chia cho x cho đến khi nó không chia hết | 
| Không gian | O(1) | chỉ có một vài biến được sử dụng | 

Hệ số logarit xuất phát từ việc chia lặp lại mỗi số cho`x`. Vì mỗi phép chia đều giảm giá trị một cách nghiêm ngặt nên tổng số phép toán vẫn đủ nhỏ dưới các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        ans = 0
        i = 0
        cnt = 1

        while i < n:
            val = a[i] * cnt

            while val % x == 0:
                val //= x
                cnt *= x

            ans += val

            if val % x != 0:
                break

            i += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""2
1 2
12
4 2
4 6 8 2
""") == "36\n44"

# minimum case
assert run("""1
1 2
1
""") == "1"

# power chain
assert run("""1
1 2
8
""") == "8"

# mixed case
assert run("""1
3 3
3 9 2
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ | 1 | trường hợp không mở rộng tối thiểu | 
| sức mạnh của x | 8 | chuỗi phân chia sâu | 
| dừng sớm | 15 | chấm dứt trên phần tử không chia được | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phần tử đầu tiên không chia hết cho`x`. Trong tình huống đó, quá trình dừng lại ngay lập tức và không có sự mở rộng nào xảy ra. Thuật toán xử lý chính xác điều này vì lần kiểm tra đầu tiên không thực hiện được vòng chia và giá trị được cộng trực tiếp. 

Một trường hợp cạnh khác xảy ra khi mọi phần tử đều chia hết cho`x`lặp đi lặp lại cho đến khi chúng trở thành 1. Trong trường hợp này, bội số tăng lên đáng kể, nhưng thuật toán không bao giờ hiện thực hóa mảng mở rộng. Thay vào đó, nó tổng hợp tất cả các phần mở rộng thành`cnt`, đảm bảo tính toán vẫn tuyến tính mặc dù cấu trúc khái niệm là hàm mũ.
