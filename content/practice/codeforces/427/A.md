---
title: "CF 427A - Tuyển Cảnh Sát"
description: "Chúng ta được cung cấp một chuỗi các sự kiện xảy ra theo thứ tự thời gian trong một thành phố. Mỗi sự kiện là sự xuất hiện của một hoặc nhiều tân binh cảnh sát hoặc sự xuất hiện của một tội phạm. Khi tân binh đến, họ sẽ tăng số lượng sĩ quan sẵn có."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 427
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 244 (Div. 2)"
rating: 800
weight: 427
solve_time_s: 82
verified: true
draft: false
---

[CF 427A - Tuyển cảnh sát](https://codeforces.com/problemset/problem/427/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các sự kiện xảy ra theo thứ tự thời gian trong một thành phố. Mỗi sự kiện là sự xuất hiện của một hoặc nhiều tân binh cảnh sát hoặc sự xuất hiện của một tội phạm. Khi tân binh đến, họ sẽ tăng số lượng sĩ quan sẵn có. Khi một tội phạm xảy ra, nó sẽ tiêu tốn chính xác một sĩ quan hiện có nếu có; nếu không thì tội đó không bị xử lý. 

Nhiệm vụ là mô phỏng quá trình này và đếm xem có bao nhiêu tội ác không thể được giao cho bất kỳ sĩ quan nào tại thời điểm chúng xảy ra. 

Kích thước đầu vào có thể lớn tới 100.000 sự kiện. Điều này ngay lập tức gợi ý rằng mọi giải pháp thực hiện quét lồng nhau hoặc xem lại các sự kiện trước đó sẽ quá chậm. Quét tuyến tính chỉ duy trì số lượng sĩ quan hiện có là đủ và cần thiết, vì mỗi sự kiện phải được xử lý theo thứ tự và các quyết định chỉ phụ thuộc vào trạng thái hiện tại. 

Một ý tưởng bạo lực cố gắng đối sánh từng tội phạm với việc tuyển dụng sớm nhất có thể hoặc tìm kiếm ngược lại các sĩ quan sẵn có sẽ biến thành hành vi bậc hai trong những trường hợp xấu nhất khi nhiều tội phạm và người được tuyển dụng xen kẽ. Với 10^5 sự kiện, O(n^2) sẽ vượt xa giới hạn khả thi. 

Một trường hợp phức tạp xuất phát từ các tội ác liên tiếp trước khi bất kỳ tân binh nào xuất hiện. Ví dụ: nếu đầu vào là`-1 -1 1`, hai tội ác đầu tiên xảy ra mà không có sĩ quan nào sẵn sàng, vì vậy cả hai đều không được điều trị. Việc tuyển dụng sau này không giúp ích gì về mặt hồi tố. 

Một trường hợp khác là khi các tân binh đến với số lượng lớn hơn mức cần thiết, chẳng hạn như`5 5 -1 -1 -1`. Chỉ có ba sĩ quan bị tiêu hao, và các sĩ quan còn lại sẽ tiếp tục tham gia các sự kiện sau này. Một cách tiếp cận tham lam sai lầm có thể "đặt lại" số lượng cho mỗi sự kiện một cách không chính xác thay vì duy trì một nhóm chung. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ cố gắng phân công rõ ràng từng sĩ quan cho từng tội phạm, có thể tìm kiếm một tuyển dụng sẵn có cho từng sự kiện tội phạm bằng cách quét các sự kiện trước đó hoặc duy trì danh sách từng sĩ quan. Mặc dù đúng về mặt logic nhưng điều này dẫn đến chi phí không cần thiết vì mỗi sĩ quan đều giống hệt nhau và có thể thay thế cho nhau. Trạng thái liên quan duy nhất là có bao nhiêu sĩ quan hiện đang được tự do. 

Nếu chúng ta mô phỏng từng sĩ quan riêng lẻ, thì mỗi người được tuyển dụng sẽ tăng một danh sách và mỗi tội phạm sẽ loại bỏ một thành phần khỏi danh sách đó. Trong trường hợp xấu nhất, chúng tôi sẽ thực hiện các thao tác danh sách O(n) cho mỗi sự kiện do dịch chuyển hoặc tìm kiếm, dẫn đến hành vi O(n^2) khi tất cả các sự kiện đều là tội phạm hoặc tân binh. 

Quan sát quan trọng là chúng ta không bao giờ cần theo dõi cấp độ danh tính của các sĩ quan. Chúng tôi chỉ cần một quầy đại diện cho các sĩ quan có sẵn. Mỗi sự kiện tuyển dụng sẽ thêm vào bộ đếm này và mỗi tội phạm sẽ sử dụng nó nếu có thể, nếu không thì sẽ tăng số lượng chưa được xử lý. Điều này làm giảm vấn đề xuống chỉ còn một lần với các bản cập nhật liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (theo dõi cá nhân) | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (mô phỏng bộ đếm) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai số nguyên: một cho các sĩ quan hiện có và một cho các tội phạm chưa được xử lý. 

Chúng tôi xử lý các sự kiện theo thứ tự. 

1. Khởi tạo một biến`available`đến 0. Điều này thể hiện số lượng sĩ quan đang rảnh rỗi vào thời điểm hiện tại. 
2. Khởi tạo một biến`untreated`về 0. Điều này theo dõi các tội phạm không thể xử lý được. 
3. Đọc từng sự kiện từ trái qua phải. 
4. Nếu giá trị sự kiện là dương, hãy tăng`available`với số tiền đó vì những tân binh sẽ có mặt ngay lập tức. 
5. Nếu giá trị sự kiện là`-1`, chúng tôi đang xử lý một tội phạm. 
6. Nếu`available`lớn hơn 0 thì giảm`available`bằng 1 kể từ khi một sĩ quan xử lý tội phạm này. 
7. Ngược lại thì tăng`untreated`bằng 1 vì không có nhân viên nào có mặt tại thời điểm này. 
8. Sau khi xử lý tất cả các sự kiện, xuất ra`untreated`. 

Quyết định ở mỗi bước phạm tội là tối ưu cục bộ vì mọi sĩ quan đều giống hệt nhau và không có lợi ích gì khi lưu hoặc lựa chọn giữa các sĩ quan. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của dãy, giá trị của`available`thể hiện chính xác bao nhiêu tội ác mà những tân binh trước đây vẫn có thể phạm phải. Vì mỗi sĩ quan có thể xử lý tối đa một tội phạm và không có giới hạn về thời hạn hoặc yêu cầu vượt quá khả năng sẵn có nên hệ thống hoạt động giống như một bộ đếm tài nguyên đơn giản. Mỗi khi tội phạm xuất hiện, việc tiêu thụ một đơn vị của quầy này luôn là tối ưu nếu có thể, và nếu không làm như vậy sẽ làm tăng số tổn thất không thể tránh khỏi đúng một lần cho mỗi lần thiếu hụt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
events = list(map(int, input().split()))

available = 0
untreated = 0

for x in events:
    if x == -1:
        if available > 0:
            available -= 1
        else:
            untreated += 1
    else:
        available += x

print(untreated)
```Mã trực tiếp tuân theo chiến lược mô phỏng. các`available`biến chỉ được cập nhật bằng phép cộng và phép trừ, đảm bảo O(1) hoạt động cho mỗi sự kiện. Thứ tự kiểm tra rất quan trọng: chúng tôi phải xử lý tội phạm trước khi cập nhật bất kỳ điều gì khác cho sự kiện đó, vì bản thân sự kiện đó là tội phạm hoặc tuyển dụng. 

Một lỗi phổ biến là lưu trữ các tân binh vào một danh sách và lấy ra từ danh sách đó. Điều đó là không cần thiết và có nguy cơ kém hiệu quả. Một sai lầm nữa là reset`available`cho mỗi sự kiện thay vì tích lũy nó trên toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
-1 -1 1
```| Sự kiện | Loại | Có sẵn | Chưa được điều trị | 
| --- | --- | --- | --- | 
| -1 | tội phạm | 0 | 1 | 
| -1 | tội phạm | 0 | 2 | 
| 1 | tuyển dụng | 1 | 2 | 

Dấu vết này cho thấy những tội phạm ban đầu không thể hưởng lợi từ những người được tuyển dụng sau này. Nhà nước phát triển nghiêm ngặt về phía trước. 

### Ví dụ 2 

đầu vào:```
6
1 -1 1 -1 -1 2
```| Sự kiện | Loại | Có sẵn | Chưa được điều trị | 
| --- | --- | --- | --- | 
| 1 | tuyển dụng | 1 | 0 | 
| -1 | tội phạm | 0 | 0 | 
| 1 | tuyển dụng | 1 | 0 | 
| -1 | tội phạm | 0 | 0 | 
| -1 | tội phạm | 0 | 1 | 
| 2 | tuyển dụng | 2 | 1 | 

Ví dụ này nhấn mạnh rằng những tân binh bổ sung sẽ tích lũy và có thể được sử dụng cho các tội ác trong tương lai, nhưng không thể khắc phục tình trạng thiếu hụt trong quá khứ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện cập nhật một số lượng biến không đổi | 
| Không gian | O(1) | Chỉ có hai bộ đếm được duy trì bất kể kích thước đầu vào | 

Quá trình quét tuyến tính dễ dàng phù hợp với giới hạn 100.000 sự kiện và việc sử dụng bộ nhớ liên tục đảm bảo không tốn phí lưu trữ lịch sử sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    events = list(map(int, sys.stdin.readline().split()))

    available = 0
    untreated = 0

    for x in events:
        if x == -1:
            if available > 0:
                available -= 1
            else:
                untreated += 1
        else:
            available += x

    return str(untreated)

# provided samples
assert run("3\n-1 -1 1\n") == "2"

# all crimes, no recruits
assert run("4\n-1 -1 -1 -1\n") == "4"

# all recruits, no crimes
assert run("5\n1 2 3 4 5\n") == "0"

# alternating pattern
assert run("6\n1 -1 1 -1 -1 2\n") == "1"

# exact balance
assert run("4\n1 1 -1 -1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả -1 | 4 | không có sĩ quan nào sẵn có | 
| tất cả đều tích cực | 0 | không xảy ra tội phạm | 
| xen kẽ | 1 | sự xen kẽ đúng đắn | 
| cân bằng | 0 | tiêu thụ tân binh hợp lý | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tội phạm xảy ra trước khi có bất kỳ tân binh nào tồn tại. Đối với đầu vào`-1 -1 -1`, thuật toán giữ`available = 0`xuyên suốt và tăng dần`untreated`cho mọi sự kiện. Điều này phù hợp với yêu cầu là không thể thực hiện chuyển nhượng có hiệu lực hồi tố. 

Một trường hợp nguy hiểm khác là khi một lượng lớn tân binh đến sau một số tội ác. Đối với đầu vào`-1 -1 5`, hai tội đầu tăng`untreated`đến 2. Khi có 5 tân binh đến,`available`trở thành 5, nhưng nó không làm giảm số lần thất bại đã được tính. Hành vi này là đúng vì tội phạm được giải quyết ngay lập tức tại thời điểm chúng xảy ra và những lỗi lầm trong quá khứ không thể sửa chữa được. 

Trường hợp cuối cùng là khi người được tuyển dụng khớp chính xác với các tội phạm ở phần sau của chuỗi, chẳng hạn như`2 -1 -1`. Bộ sự kiện đầu tiên`available = 2`. Mỗi tội ác tiêu tốn một sĩ quan, giảm số đếm xuống 0 và không có tội phạm nào bị bỏ sót. Điều này xác nhận rằng bộ đếm mô hình chính xác phép gán một đối một.
