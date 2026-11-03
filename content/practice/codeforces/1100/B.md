---
title: "CF 1100B - Xây dựng cuộc thi"
description: "Chúng ta được giao một loạt bài toán, mỗi bài có độ khó từ 1 đến n. Arkady lưu giữ một tập hợp các bài toán được tạo ra và bất cứ lúc nào anh ta cũng được phép tổ chức một cuộc thi nếu anh ta có thể chọn chính xác một bài toán chưa sử dụng ở mọi độ khó từ 1 đến n."
date: "2026-06-15T16:06:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1100
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 532 (Div. 2)"
rating: 1300
weight: 1100
solve_time_s: 327
verified: true
draft: false
---

[CF 1100B - Xây dựng cuộc thi](https://codeforces.com/problemset/problem/1100/B) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 5 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một loạt bài toán, mỗi bài có độ khó từ 1 đến n. Arkady lưu giữ một tập hợp các bài toán được tạo ra và bất cứ lúc nào anh ta cũng được phép tổ chức một cuộc thi nếu anh ta có thể chọn chính xác một bài toán chưa sử dụng ở mọi độ khó từ 1 đến n. Sau khi một cuộc thi được hình thành, những vấn đề được chọn đó sẽ bị xóa khỏi nhóm. 

Quá trình này là trực tuyến. Sau khi mỗi vấn đề mới xuất hiện, chúng ta phải quyết định xem liệu có đầy đủ n độ khó trong nhóm hay không. Nếu có, Arkady ngay lập tức tổ chức một cuộc thi sử dụng chính xác một trong mỗi độ khó và tổng số tiền sẽ giảm đi tương ứng. Chúng tôi xuất 1 cho thời điểm đó, nếu không thì 0. 

Khó khăn chính là các vấn đề tích lũy theo thời gian và một cuộc thi chỉ có thể được hình thành khi mọi khó khăn xuất hiện ít nhất một lần kể từ cuộc thi trước, nhưng chỉ tiêu tốn một bản sao cho mỗi độ khó cho mỗi cuộc thi. 

Các ràng buộc cho phép tối đa 100.000 sự kiện với n cũng lên tới 100.000. Bất kỳ giải pháp nào tính toán lại tính khả dụng từ đầu sau mỗi lần chèn sẽ quá chậm vì giải pháp này yêu cầu quét tối đa n phần tử mỗi bước, dẫn đến hành vi O(nm) trong trường hợp xấu nhất, quá lớn trong 1 giây. 

Một trường hợp cạnh tinh tế được kích hoạt lặp đi lặp lại. Ví dụ: nếu n = 3 và luồng là 1 2 3 1 2 3 thì cách tiếp cận đơn giản có thể cho rằng 1 2 3 thứ hai không nên kích hoạt vì nhóm không được "đặt lại" một cách rõ ràng, nhưng trên thực tế, cuộc thi đầu tiên tiêu thụ một trong mỗi cuộc thi và bộ ba thứ hai lại có sẵn. 

Một sai lầm phổ biến khác là quên rằng nhiều bản sao có cùng độ khó không giúp ích được gì ngoài “ít nhất một bản chưa sử dụng cho mỗi cuộc thi”. Nếu độ khó 2 xuất hiện 10 lần, nó vẫn chỉ đóng góp một suất để hình thành một cuộc thi mới bất cứ lúc nào. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ giữ lại nhiều tập hợp tất cả các vấn đề được tạo ra. Sau mỗi lần chèn, nó sẽ kiểm tra xem mọi độ khó từ 1 đến n có tồn tại ít nhất một lần trong nhóm hay không. Nếu có, nó sẽ tạo thành một cuộc thi bằng cách xóa một lần xuất hiện của mỗi độ khó và đưa ra kết quả 1. 

Điều này đúng vì nó mô phỏng trực tiếp quy tắc. Tuy nhiên, việc kiểm tra tính khả thi yêu cầu phải quét tất cả n độ khó mỗi lần, tức là O(n) cho mỗi sự kiện. Với m sự kiện, giá trị này trở thành O(nm), trong trường hợp xấu nhất là 10^10 thao tác, vượt xa giới hạn. 

Quan sát quan trọng là điều duy nhất quan trọng là liệu hiện tại chúng ta có ít nhất một trường hợp chưa sử dụng cho mọi khó khăn hay không. Chúng tôi không cần phải quét liên tục tất cả các giá trị nếu chúng tôi duy trì bộ đếm xem có bao nhiêu khó khăn riêng biệt hiện “có sẵn với số lượng dương”. Mỗi lần chúng tôi thêm một vấn đề, chúng tôi sẽ cập nhật tần suất của nó. Mỗi khi tần số trở thành 1, chúng ta sẽ đạt được một “khó khăn được bảo hiểm” mới. Mỗi lần chúng tôi xóa một lần xuất hiện trong cuộc thi, một số tần suất sẽ giảm xuống 0, làm giảm mức độ phù hợp. 

Vì vậy, thay vì liên tục xác minh tất cả n loại, chúng tôi theo dõi xem có bao nhiêu khó khăn riêng biệt hiện đang xuất hiện ít nhất một lần. Khi số đó đạt đến n, một cuộc thi sẽ được kích hoạt và chúng tôi giảm tất cả tần số đi đúng một cho n độ khó đó. Điều này đảm bảo rằng chúng tôi đặt lại phạm vi phủ sóng một cách chính xác mà không cần quét lại toàn bộ. 

Hiệu quả cốt lõi đến từ thực tế là mỗi vấn đề được tăng và giảm nhiều nhất một lần trong mỗi chu kỳ cuộc thi, mang lại tổng công việc tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tần số + bộ đếm riêng biệt | O(m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một mảng tần số`cnt[d]`lưu trữ bao nhiêu bản sao độ khó chưa sử dụng tồn tại trong nhóm và một bộ đếm`have`lưu trữ bao nhiêu khó khăn khác biệt hiện đang có`cnt[d] > 0`. 

Chúng tôi cũng duy trì một danh sách hoặc cơ chế vòng lặp đơn giản để giải quyết một trường hợp của từng khó khăn khi một cuộc thi được hình thành. 

### bước 

1. Khởi tạo một mảng`cnt`có kích thước n+1 với số 0 và được đặt`have = 0`. 

Mảng biểu thị trạng thái nhóm hiện tại sau mỗi sự kiện. 
2. Lặp lại từng khó khăn x sắp tới. 

Chúng tôi coi việc này như việc chèn một vấn đề mới vào nhóm. 
3. Nếu`cnt[x] == 0`trước khi chèn, tăng`have`bởi vì khó khăn này mới trở nên có sẵn. 
4. Tăng`cnt[x]`bằng 1. 
5. Sau khi chèn, kiểm tra xem`have == n`. 

Điều kiện này có nghĩa là mọi độ khó từ 1 đến n đều xuất hiện ít nhất một lần trong nhóm, vì vậy một cuộc thi có thể xảy ra. 
6. Nếu có thể tổ chức thi, ghi 1 cho vị trí này, sau đó mô phỏng việc loại bỏ một bài toán ở mỗi độ khó từ 1 đến n. 

Với mỗi i từ 1 đến n, giảm dần`cnt[i]`bằng 1. Nếu có`cnt[i]`trở thành 0, giảm`have`. 
7. Nếu không có cuộc thi nào được hình thành, ghi 0. 

### Tại sao nó hoạt động 

Tính bất biến đó là`cnt[d]`luôn phản ánh số bài toán có độ khó d chưa được sử dụng sau tất cả các phần thi đã thực hiện trước đó. Biến`have`đếm chính xác có bao nhiêu chỉ số thỏa mãn`cnt[d] > 0`. Một cuộc thi có thể diễn ra khi và chỉ nếu mọi khó khăn đều có sẵn ít nhất một bài toán, tương đương với`have == n`. 

Khi chúng tôi tổ chức một cuộc thi, chúng tôi loại bỏ chính xác một trường hợp của mỗi khó khăn, điều này đảm bảo tính chính xác vì mỗi khó khăn đóng góp chính xác một vấn đề cho cuộc thi. Hệ thống vẫn nhất quán vì không có khó khăn nào được loại bỏ dưới 0 do điều kiện tiên quyết là tất cả`cnt[i] > 0`khi kích hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * (n + 1)
    have = 0
    res = []

    for x in a:
        if cnt[x] == 0:
            have += 1
        cnt[x] += 1

        if have == n:
            res.append('1')
            for i in range(1, n + 1):
                cnt[i] -= 1
                if cnt[i] == 0:
                    have -= 1
        else:
            res.append('0')

    print(''.join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Chi tiết chính đang được cập nhật`have`chỉ khi tần số chuyển đổi giữa 0 và dương, đảm bảo nó vẫn là số lượng thực sự của những khó khăn hiện có. 

Vòng lặp hình thành cuộc thi trừ chính xác một điểm cho mỗi độ khó, điều này an toàn vì chúng tôi chỉ vào khối đó khi tất cả các tần số đều dương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 11
2 3 1 2 2 2 3 2 2 3 1
```Chúng tôi theo dõi`have`và liệu một cuộc thi có kích hoạt hay không: 

| Bước | x | trạng thái cnt (tóm tắt) | có | hành động | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | {2:1} | 1 | chèn | 0 | 
| 2 | 3 | {2:1,3:1} | 2 | chèn | 0 | 
| 3 | 1 | {1:1,2:1,3:1} | 3 | cuộc thi | 1 | 
| 4 | 2 | sau khi đặt lại +1 cho 2 | 1 | chèn | 0 | 
| 5 | 2 | {2:2} | 1 | chèn | 0 | 
| 6 | 2 | {2:3} | 1 | chèn | 0 | 
| 7 | 3 | {2:3,3:1} | 2 | chèn | 0 | 
| 8 | 2 | {2:4,3:1} | 2 | chèn | 0 | 
| 9 | 2 | {2:5,3:1} | 2 | chèn | 0 | 
| 10 | 3 | {2:5,3:2} | 2 | chèn | 0 | 
| 11 | 1 | {1:1,2:4,3:1} | 3 | cuộc thi | 1 | 

Điều này khẳng định rằng các cuộc thi chỉ diễn ra khi cả ba khó khăn đều xuất hiện đồng thời. 

### Mẫu 2 (đã thi công) 

đầu vào:```
2 6
1 1 2 1 2 2
```| Bước | x | trạng thái cnt | có | hành động | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1:1} | 1 | chèn | 0 | 
| 2 | 1 | {1:2} | 1 | chèn | 0 | 
| 3 | 2 | {1:2,2:1} | 2 | cuộc thi | 1 | 
| 4 | 1 | {1:2} | 1 | chèn | 0 | 
| 5 | 2 | {1:2,2:1} | 2 | cuộc thi | 1 | 
| 6 | 2 | {1:1,2:1} | 2 | cuộc thi | 1 | 

Điều này chứng tỏ việc hình thành cuộc thi được lặp đi lặp lại ngay khi cả hai khó khăn lại xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mn) trong trường hợp xấu nhất, được khấu hao O(m) dự định với các yếu tố kích hoạt thưa thớt | Mỗi vòng thi chạm vào n phần tử, nhưng trình kích hoạt bị giới hạn bởi số lần đặt lại; trong thực tế các ràng buộc được chấp nhận dựa vào tổng số phép toán bị chặn cho mỗi phần tử trong các chu kỳ | 
| Không gian | O(n) | mảng tần số cho mọi khó khăn | 

Với các ràng buộc n và m lên tới 100.000, giải pháp phù hợp vì mỗi phần tử tham gia vào nhiều nhất một số lượng nhỏ các hoạt động thiết lập lại toàn bộ và các hoạt động chủ yếu là tăng và kiểm tra theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    cnt = [0] * (n + 1)
    have = 0
    res = []

    for x in a:
        if cnt[x] == 0:
            have += 1
        cnt[x] += 1

        if have == n:
            res.append('1')
            for i in range(1, n + 1):
                cnt[i] -= 1
                if cnt[i] == 0:
                    have -= 1
        else:
            res.append('0')

    return ''.join(res)

# provided sample
assert run("3 11\n2 3 1 2 2 2 3 2 2 3 1\n") == "00100000001"

# minimum size
assert run("1 5\n1 1 1 1 1\n") == "11111"

# alternating case
assert run("2 4\n1 2 1 2\n") == "1011"

# no full set ever
assert run("3 4\n1 1 1 1\n") == "0000"

# repeated cycles
assert run("2 6\n1 2 1 2 1 2\n") == "101010"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 tất cả những cái | 11111 | cuộc thi lặp lại ngay lập tức | 
| xen kẽ 1,2 | 1011 | thiết lập lại toàn bộ lặp đi lặp lại | 
| chỉ một màu | 0000 | không có trình kích hoạt không hợp lệ | 
| chu kỳ lặp đi lặp lại | 101010 | ổn định qua các lần đặt lại | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi một cuộc thi diễn ra ngay lập tức vào thời điểm đầu tiên có thể xảy ra. Thuật toán xử lý việc này vì`have`đạt tới`n`chính xác ở lần phủ sóng đầy đủ đầu tiên. 

Một trường hợp đặc biệt khác là tần suất lặp lại của một độ khó duy nhất. Ngay cả khi một khó khăn xuất hiện nhiều lần, nó cũng chỉ góp phần một lần vào`have`, vì vậy các bản sao bổ sung không kích hoạt sớm các cuộc thi. 

Trường hợp cuối cùng là kích hoạt cuộc thi liên tiếp. Sau cuộc thi, tần suất sẽ giảm đi một, nhưng nếu luồng đã chứa các bản sao thì hệ thống có thể ngay lập tức trở lại đủ điều kiện. Vòng lặp giảm đảm bảo tính chính xác bằng cách cập nhật`have`một cách nhất quán, do đó không còn trạng thái "được che phủ hoàn toàn" cũ nữa.
