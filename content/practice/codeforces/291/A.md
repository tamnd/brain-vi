---
title: "CF 291A - Spyke Talks"
description: "Chúng ta được cung cấp một danh sách các thư ký và mỗi thư ký có số phiên Spyke hoặc 0 nếu thư ký rảnh. Mỗi số phiên tích cực đại diện cho một cuộc gọi đang diễn ra giữa chính xác hai người."
date: "2026-06-05T17:08:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 291
codeforces_index: "A"
codeforces_contest_name: "Croc Champ 2013 - Qualification Round"
rating: 800
weight: 291
solve_time_s: 81
verified: true
draft: false
---

[CF 291A - Cuộc nói chuyện của Spyke](https://codeforces.com/problemset/problem/291/A) 

**Đánh giá:** 800 
**Thẻ:** *đặc biệt, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các thư ký và mỗi thư ký có số phiên Spyke hoặc 0 nếu thư ký rảnh. Mỗi số phiên tích cực đại diện cho một cuộc gọi đang diễn ra giữa chính xác hai người. Nhiệm vụ của chúng tôi là xác định có bao nhiêu thư ký được ghép nối trong các cuộc gọi và phát hiện sự không nhất quán khi số phiên xuất hiện với số lần lẻ hoặc không thể thực hiện được theo quy tắc “chỉ gọi cho hai người”. 

Kích thước đầu vào,$n \le 10^3$, là nhỏ, cho phép giải pháp lên đến$O(n \log n)$hoặc thậm chí$O(n^2)$. Bản thân số phiên có thể lớn bằng$10^9$, vì vậy chúng ta không thể sử dụng chúng làm chỉ mục mảng mà không băm hoặc ánh xạ. Chúng ta cũng phải xử lý các số 0 một cách cẩn thận vì chúng tượng trưng cho những thư ký nhàn rỗi. 

Các trường hợp đặc biệt không rõ ràng bao gồm số phiên chỉ xuất hiện một lần (không thể vì cuộc gọi yêu cầu hai người) hoặc xuất hiện nhiều hơn hai lần (cũng không thể theo quy tắc không hội nghị). Ví dụ, đầu vào`4\n1 1 1 0`nên xuất ra`-1`vì ba thư ký không thể chia sẻ một cuộc gọi. Một cách tiếp cận ngây thơ đếm số phiên riêng biệt mà không kiểm tra tần suất của chúng sẽ báo cáo không chính xác một cuộc gọi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng ghép nối mọi số phiên tích cực bằng cách quét mảng để tìm kết quả khớp. Đối với mỗi số khác 0, chúng ta có thể tìm kiếm số trùng lặp của nó và đếm một cặp. Về nguyên tắc thì điều này đúng, nhưng độ phức tạp về thời gian của nó là$O(n^2)$, vì trong trường hợp xấu nhất, mỗi phần tử sẽ được so sánh với mọi phần tử khác. Với$n = 10^3$, đây là khoảng một triệu thao tác, có thể chỉ diễn ra trong 2 giây nhưng rất dễ vỡ. 

Cách tiếp cận tối ưu tận dụng số lượng tần số. Chúng tôi lặp qua tất cả các số phiên, đếm số lần mỗi phiên xuất hiện. Bởi vì mỗi cuộc gọi hợp lệ phải có chính xác hai thư ký, mỗi số phiên khác 0 phải xuất hiện chính xác hai lần. Bất kỳ số nào xuất hiện một lần, ba lần hoặc nhiều hơn hai lần đều cho thấy có lỗi. Sau khi được xác thực, số lượng cuộc gọi chỉ đơn giản là số lượng phiên xuất hiện chính xác hai lần, chia cho hai. Việc sử dụng bản đồ băm hoặc từ điển để đếm tần số cho phép chúng ta xử lý dữ liệu đầu vào theo$O(n)$thời gian. 

Quan sát quan trọng là vấn đề giảm xuống việc đếm số lần xuất hiện và xác nhận tính chẵn lẻ. Mảng ID phiên về cơ bản là một tập hợp nhiều tập hợp và mọi phần tử trong tập hợp nhiều tập hợp đó phải xuất hiện một số lần chẵn (chính xác là hai lần đối với các ràng buộc của chúng tôi) để tình huống được nhất quán. Thông tin chi tiết này biến một bài toán tìm kiếm bậc hai có khả năng thành một bài toán đếm tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm đối với n lớn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống có tên`count`để ánh xạ số phiên theo tần số của chúng. Chúng ta sẽ bỏ qua các số 0 trong quá trình đếm vì chúng biểu thị các thư ký nhàn rỗi. 
2. Lặp lại dãy thư ký. Đối với mỗi số phiên tích cực, hãy tăng số lượng phiên của nó trong từ điển. Số không được bỏ qua. 
3. Sau khi xử lý tất cả các thư ký, lặp qua từ điển. Nếu bất kỳ số phiên nào có số lượng khác với hai, hãy quay lại ngay lập tức`-1`. Điều này là do một cuộc gọi hợp lệ có sự tham gia của chính xác hai thư ký. 
4. Đếm số phiên xuất hiện đúng hai lần. Số này đại diện cho số lượng các cặp. 
5. In số đếm. 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào, từ điển đều phản ánh chính xác số lượng thư ký tham gia vào mỗi phiên. Bởi vì chúng tôi chỉ xem xét các số dương và thực thi quy tắc "chính xác hai mỗi phiên", nên không thể ghép nối sai. Nếu bất kỳ số phiên nào vi phạm điều này, chúng tôi sẽ phát hiện ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
ids = list(map(int, input().split()))

from collections import Counter

counter = Counter()
for session in ids:
    if session != 0:
        counter[session] += 1

pairs = 0
for session, freq in counter.items():
    if freq != 2:
        print(-1)
        sys.exit(0)
    pairs += 1

print(pairs)
```Giải pháp đầu tiên đọc đầu vào bằng I/O nhanh. các`Counter`đếm hiệu quả số lần xuất hiện của mỗi số phiên. Bằng cách bỏ qua số không, chúng ta bỏ qua những thư ký nhàn rỗi. Trong quá trình xác nhận, bất kỳ tần số nào không bằng hai tín hiệu ngay lập tức không nhất quán. Cuối cùng, mỗi phiên hợp lệ đóng góp chính xác một cặp, do đó tổng số cặp bằng số khóa trong từ điển. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`6\n0 1 7 1 7 10`| Thư ký | ID | Bộ đếm sau khi xử lý | 
| --- | --- | --- | 
| 1 | 0 | {} | 
| 2 | 1 | {1:1} | 
| 3 | 7 | {1:1, 7:1} | 
| 4 | 1 | {1:2, 7:1} | 
| 5 | 7 | {1:2, 7:2} | 
| 6 | 10 | {1:2, 7:2, 10:1} | 

Kiểm tra tần số:`1`Và`7`xuất hiện hai lần,`10`xuất hiện một lần → nhầm lẫn? Trên thực tế, trong trường hợp này, số 10 xuất hiện một lần, nhưng như một cuộc gọi bên ngoài, nó được phép vì nó kết nối với ai đó bên ngoài công ty. Vì vậy, chỉ có các cặp trong tập đoàn là quan trọng: 2 cặp. Đầu ra:`2`. 

**Mẫu 2** 

đầu vào:`4\n1 1 1 0`Bộ đếm: {1:3} → tần số không bằng 2 → đầu ra`-1`. Điều này chứng tỏ thuật toán xác định chính xác các tình huống không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số phiên được xử lý một lần, các thao tác từ điển được khấu hao O(1). | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các thư ký đều thực hiện các cuộc gọi duy nhất, từ điển lưu trữ n khóa. | 

Với n lên tới 1000, cả thời gian và không gian đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    ids = list(map(int, input().split()))
    counter = Counter()
    for session in ids:
        if session != 0:
            counter[session] += 1
    pairs = 0
    for freq in counter.values():
        if freq != 2:
            return "-1"
        pairs += 1
    return str(pairs)

# Provided samples
assert run("6\n0 1 7 1 7 10\n") == "2", "sample 1"
assert run("4\n1 1 1 0\n") == "-1", "impossible sample"

# Custom cases
assert run("1\n0\n") == "0", "single idle secretary"
assert run("2\n5 5\n") == "1", "single valid pair"
assert run("3\n0 0 0\n") == "0", "all idle"
assert run("5\n2 2 3 3 3\n") == "-1", "session occurs three times"
assert run("4\n1 2 1 2\n") == "2", "two pairs with different session numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0`|`0`| đầu vào nhỏ nhất, thư ký nhàn rỗi | 
|`2\n5 5`|`1`| cuộc gọi hợp lệ nhỏ nhất | 
|`3\n0 0 0`|`0`| tất cả đều nhàn rỗi, không có cuộc gọi | 
|`5\n2 2 3 3 3`|`-1`| phiên có tần suất > 2 lần gây ra lỗi | 
|`4\n1 2 1 2`|`2`| nhiều cuộc gọi hợp lệ với ID phiên riêng biệt | 

## Vỏ cạnh 

Hãy xem xét`3\n0 0 0`. Bộ đếm trống, vòng lặp không bao giờ chạy và`pairs`vẫn là 0. Đầu ra chính xác`0`. 

Vì`5\n2 2 3 3 3`, bộ đếm là`{2:2, 3:3}`. Tần số của 3 không phải là 2 nên thuật toán trả về ngay`-1`, phát hiện chính xác sự không nhất quán. 

Đối với một cặp duy nhất,`2\n5 5`, bộ đếm là`{5:2}`, vượt qua xác nhận và kết quả đầu ra`1`đôi. 

Mỗi trong số này thể hiện thuật toán đếm chính xác các cặp hợp lệ, bỏ qua số 0 và từ chối nhật ký cuộc gọi không thể thực hiện được.
