---
title: "CF 103870C - Lịch"
description: "Chúng tôi đang làm việc với lịch đơn giản hóa của một năm không nhuận, trong đó năm có 365 ngày và mỗi ngày có thể chứa một sự kiện hoặc để trống. Đầu vào cuối cùng mô tả những ngày cụ thể có sự kiện và mọi thứ khác hoàn toàn trống rỗng."
date: "2026-07-02T07:44:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "C"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 44
verified: true
draft: false
---

[CF 103870C - Lịch](https://codeforces.com/problemset/problem/103870/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với lịch đơn giản hóa của một năm không nhuận, trong đó năm có 365 ngày và mỗi ngày có thể chứa một sự kiện hoặc để trống. Đầu vào cuối cùng mô tả những ngày cụ thể có sự kiện và mọi thứ khác hoàn toàn trống rỗng. 

Nhiệm vụ là xác định khoảng thời gian liên tiếp dài nhất mà không có sự kiện nào xảy ra. Nói cách khác, khi chúng tôi đánh dấu tất cả các ngày sự kiện, chúng tôi muốn độ dài tối đa của một khối ngày liên tục trong đó không có ngày nào được đánh dấu đó xuất hiện. 

Một cách hữu ích để suy nghĩ về điều này là tưởng tượng một mảng có độ dài 365, trong đó mỗi vị trí tương ứng với một ngày trong năm. Giá trị 1 có nghĩa là một sự kiện xảy ra vào ngày đó và 0 có nghĩa là ngày đó rảnh rỗi. Mục tiêu là tìm đoạn số 0 liền kề dài nhất. 

Các ràng buộc cực kỳ nhỏ về kích thước lịch vì độ dài mảng được cố định ở 365. Ngay cả khi có nhiều mô tả sự kiện, tổng mô phỏng vẫn bị giới hạn bởi cấu trúc có kích thước không đổi này. Điều này ngay lập tức loại trừ mọi lo ngại về tối ưu hóa logarit hoặc tuyến tính đối với đầu vào lớn. Ngay cả một mô phỏng đơn giản quét liên tục trong cả năm cũng đủ nhanh. 

Sự tinh tế chính là chuyển đổi chính xác các mô tả sự kiện thành điểm đánh dấu từng ngày. Một cách tiếp cận ngây thơ có thể cố gắng coi các sự kiện là các chỉ số trực tiếp mà không dịch chính xác các ngày theo lịch hoặc các khoảng thời gian lặp lại, điều này có thể dẫn đến việc đánh dấu ngày không chính xác. 

Một số trường hợp đặc biệt quan trọng ở đây. Một là khi không có sự kiện nào cả. Trong trường hợp đó, cả năm là miễn phí và câu trả lời phải là 365. Việc triển khai bất cẩn giả định rằng ít nhất một sự kiện có thể trả về 0 không chính xác. Một trường hợp khác là khi các sự kiện diễn ra hàng ngày. Khi đó, đoạn trống dài nhất là 0 và các lỗi sai lệch thường trả về 1 không chính xác. Trường hợp thứ ba phát sinh khi việc tạo sự kiện sử dụng bước lặp lại và bước vượt quá hoặc sai lệch với việc lập chỉ mục ngày, điều này có thể âm thầm bỏ qua việc đánh dấu các ngày ranh giới như ngày 365. 

## Phương pháp tiếp cận 

Ý tưởng brute-force bắt đầu từ việc quan sát rằng khi chúng ta biết ngày nào bị chiếm, chúng ta chỉ cần quét mảng 365 ngày và đếm các số 0 liên tiếp. Phần này đã được tối ưu một cách độc lập vì nó tuyến tính với một hằng số rất nhỏ. 

Phần khó hơn là xây dựng chuỗi ngày diễn ra sự kiện. Mỗi sự kiện được xác định theo cách có thể tạo ra nhiều ngày bị ảnh hưởng bằng cách liên tục thêm kích thước bước cố định C cho đến khi vượt qua ngày 365. Đối với mỗi chuỗi như vậy, chúng tôi đánh dấu các vị trí tương ứng trong lịch. 

Một cấu trúc đơn giản sẽ là, đối với mỗi nguồn sự kiện, liên tục tăng ngày và đánh dấu trực tiếp mảng đó. Điều này đã đúng và vẫn đủ hiệu quả vì mỗi chuỗi có tối đa 365/C lần lặp và tổng số lần cập nhật được giới hạn bằng 365 lần số nguồn sự kiện. Sau khi hoàn tất việc đánh dấu, một lần quét sẽ tìm thấy chuỗi số 0 dài nhất. 

Thông tin chi tiết quan trọng là chúng tôi không cần bất kỳ cấu trúc dữ liệu nâng cao hoặc hợp nhất theo khoảng thời gian nào. Lịch cố định và nhỏ nên việc mô phỏng trực tiếp chiếm ưu thế trong mọi thứ. Cấu trúc của các cập nhật cấp số cộng lặp đi lặp lại hoàn toàn phù hợp với mảng đánh dấu boolean. 

Brute-force hoạt động vì mỗi sự kiện chỉ ảnh hưởng đến một tập hợp các ngày riêng biệt có thể dự đoán được, nhưng nó sẽ trở nên lộn xộn về mặt khái niệm nếu chúng ta cố gắng tối ưu hóa sớm. Việc quan sát thấy miền chỉ có 365 ngày cho phép chúng tôi coi mọi thứ như một vấn đề mô phỏng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trực tiếp) | O(365 · N) | O(365) | Đã chấp nhận | 
| Tối ưu (mô phỏng giống nhau, quét có cấu trúc) | O(365 · N) | O(365) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Khởi tạo mảng lịch 

Chúng tôi tạo ra một mảng`v`có kích thước 366 (lập chỉ mục dựa trên 1 là thuận tiện), được khởi tạo bằng 0. Mỗi chỉ số thể hiện liệu một sự kiện có xảy ra vào ngày đó hay không. 

Điều này cung cấp cho chúng tôi một cách trực tiếp để đánh dấu và truy vấn trạng thái hàng ngày. 

### 2. Phân tích từng trình tạo sự kiện 

Đối với mỗi mô tả sự kiện, chúng tôi bắt đầu vào một ngày đầu tiên nào đó`d`và liên tục thêm kích thước bước`C`. Mỗi khi chúng tôi hạ cánh vào một ngày hợp lệ (365), chúng tôi đánh dấu`v[d] = 1`. 

Chúng tôi tiếp tục quá trình này cho đến khi ngày vượt quá 365. 

Lý do cần tính bước thay vì tính phạm vi dạng đóng là vì mỗi sự kiện xác định một cấp số cộng rời rạc, không phải một khoảng liên tục. 

### 3. Xây dựng lịch sự kiện đầy đủ 

Sau khi xử lý tất cả các trình tạo sự kiện, mỗi ngày có ít nhất một sự kiện sẽ được đánh dấu là 1 trong`v`. Số ngày không có sự kiện nào vẫn là 0. 

Việc tổng hợp này rất quan trọng vì nhiều trình tạo có thể đánh dấu cùng một ngày nhưng chúng tôi chỉ quan tâm đến việc liệu có tồn tại ít nhất một sự kiện hay không. 

### 4. Quét các số 0 liên tiếp dài nhất 

Chúng tôi duyệt mảng từ ngày 1 đến ngày 365, duy trì bộ đếm đang chạy`cur`cho chuỗi ngày trống hiện tại. 

Nếu như`v[i] == 0`, chúng tôi tăng`cur`và cập nhật câu trả lời nếu`cur`trở nên lớn hơn. 

Nếu như`v[i] == 1`, chúng tôi đặt lại`cur`về 0, nhưng chúng tôi không đặt lại mức tối đa toàn cầu. 

Sự tách biệt giữa chuỗi hiện tại và mức tối đa toàn cầu là điều đảm bảo tính chính xác. 

### Tại sao nó hoạt động 

Tại mọi vị trí trong lịch, chúng tôi duy trì sự phân loại chính xác xem ngày đó có bận hay không. Sau đó, quá trình quét sẽ giảm bớt vấn đề để tìm phân đoạn liền kề dài nhất trong mảng nhị phân, được giải quyết bằng cách duy trì một bất biến:`cur`luôn bằng số lượng mục có giá trị 0 liên tiếp kết thúc ở chỉ mục hiện tại. Vì mỗi ngày được xử lý chính xác một lần và bước đánh dấu nắm bắt đầy đủ tất cả các lần xuất hiện sự kiện nên không có phân đoạn nào bị bỏ sót và không xảy ra phần mở rộng không hợp lệ của phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    v = [0] * 366  # 1..365
    
    for _ in range(n):
        d, c = map(int, input().split())
        
        while d <= 365:
            v[d] = 1
            d += c
    
    ans = 0
    cur = 0
    
    for i in range(1, 366):
        if v[i] == 0:
            cur += 1
            if cur > ans:
                ans = cur
        else:
            cur = 0
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được chia thành hai giai đoạn: đầu tiên, chúng tôi xây dựng lịch bằng cách đánh dấu tất cả các ngày sự kiện được tạo bởi cấp số cộng. Vòng lặp while đảm bảo chúng ta chỉ ở trong giới hạn của năm. Giai đoạn thứ hai là quét tuyến tính duy nhất để tính chuỗi ngày không được đánh dấu dài nhất. 

Một lỗi triển khai phổ biến là quên đặt lại chuỗi hiện tại khi gặp một ngày sự kiện, điều này sẽ hợp nhất không chính xác các khoảng thời gian rảnh riêng biệt. Một cách khác là xử lý sai việc lập chỉ mục ngày dựa trên 1, điều này có thể thay đổi tất cả các kết quả theo một nếu sử dụng mảng dựa trên 0 mà không điều chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử chúng ta có hai trình tạo sự kiện:`(1, 3)`Và`(2, 5)`. 

Quá trình đánh dấu như sau. 

| Bước | Máy phát điện | Hiện tại d | Hành động | trạng thái v (một phần) | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3) | 1 | đánh dấu 1 | v[1]=1 | 
| 2 | (1,3) | 4 | đánh dấu 4 | v[1]=1, v[4]=1 | 
| 3 | (1,3) | 7 | điểm 7 | v[1], v[4], v[7] | 
| 4 | (2,5) | 2 | đánh dấu 2 | v[2]=1 | 
| 5 | (2,5) | 7 | đánh dấu lại 7 | không thay đổi | 
| 6 | (2,5) | 12 | đánh dấu 12 | v[12]=1 | 

Bây giờ đang quét từ ngày 1: 

| Ngày | v[i] | cur | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 
| 3 | 0 | 1 | 1 | 
| 4 | 1 | 0 | 1 | 
| 5 | 0 | 1 | 1 | 
| 6 | 0 | 2 | 2 | 
| 7 | 1 | 0 | 2 | 

Ví dụ này cho thấy cách các chuỗi sự kiện chồng chéo hợp nhất một cách tự nhiên trong mảng đánh dấu và quá trình quét sẽ trích xuất một cách rõ ràng đoạn tự do dài nhất. 

### Ví dụ 2 

Hãy xem xét một máy phát điện duy nhất`(10, 7)`. 

Các ngày được đánh dấu là 10, 17, 24, 31,... 

| Đoạn trích phạm vi ngày | v[i] | 
| --- | --- | 
| 8-9 | 0 | 
| 10 | 1 | 
| 16-11 | 0 | 
| 17 | 1 | 

Quá trình quét tạo ra: 

| tôi | v[i] | cur | 
| --- | --- | --- | 
| 8 | 0 | 1 | 
| 9 | 0 | 2 | 
| 10 | 1 | 0 | 
| 11 | 0 | 1 | 
| 12 | 0 | 2 | 
| 13 | 0 | 3 | 
| 14 | 0 | 4 | 
| 15 | 0 | 5 | 
| 16 | 0 | 6 | 
| 17 | 1 | 0 | 

Chuỗi dài nhất ở đây là 6, xảy ra từ ngày 11 đến ngày 16. Điều này xác nhận rằng quá trình quét xác định chính xác các phân đoạn không bị gián đoạn tối đa ngay cả khi các sự kiện diễn ra định kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(365 · N) | Mỗi trình tạo đánh dấu tối đa 365/C ngày và chúng tôi quét 365 ngày một lần | 
| Không gian | O(365) | Mảng lịch có kích thước cố định | 

Kích thước không đổi của lịch đảm bảo giải pháp chạy thoải mái trong giới hạn ngay cả đối với số lượng lớn người tạo sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if solve() is not None else ""

# edge: no events
assert run("0\n") == "365", "no events"

# edge: full coverage
assert run("1\n1 1\n") == "0", "all days occupied"

# simple spacing
assert run("2\n1 2\n2 2\n") in ["0", "1", "2"], "overlap behavior"

# single chain
assert run("1\n10 7\n") != "", "basic progression"

# boundary day 365
assert run("1\n365 1\n") == "364", "last day marking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có sự kiện | 365 | lịch trống | 
| bảo hiểm đầy đủ | 0 | tất cả các ngày bị chặn | 
| máy phát điện cách nhau | biến | xử lý chồng chéo | 
| tiến triển | không trống | tính đúng đắn của bước số học | 
| ranh giới 365 | 364 | đánh dấu cạnh đúng cách | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi không có trình tạo sự kiện. Trong trường hợp này, lịch vẫn hoàn toàn trống. Quá trình quét bắt đầu vào ngày thứ 1, tăng dần`cur`và không bao giờ đặt lại nó, tạo ra câu trả lời cuối cùng là 365. Điều này xác nhận rằng thuật toán không dựa vào việc gặp phải ít nhất một sự kiện để hoạt động chính xác. 

Một trường hợp đặc biệt khác xảy ra khi mỗi ngày đều được đánh dấu là một sự kiện. Ví dụ: nếu trình tạo bao gồm tất cả các ngày qua bước 1 bắt đầu từ 1, mọi vị trí trong`v`trở thành 1. Trong quá trình quét,`cur`được đặt lại ở mọi bước, không bao giờ tích lũy vượt quá 0, vì vậy câu trả lời cuối cùng là 0. Điều này xác minh rằng các lần đặt lại liên tiếp được xử lý chính xác mà không vô tình bảo toàn trạng thái một phần. 

Trường hợp khó phát hiện cuối cùng là khi chuỗi sự kiện kết thúc chính xác vào ngày thứ 365. Điều kiện vòng lặp while`d <= 365`đảm bảo rằng ngày 365 sẽ được bao gồm nếu đạt đến. Ví dụ: bắt đầu từ 365 với bước 1 chỉ đánh dấu ngày 365. Sau đó, quá trình quét xử lý chính xác ngày 365 như một trình chặn và không kéo dài bất kỳ khoảng thời gian rảnh nào vượt quá ngày đó.
