---
title: "CF 102277H - Sắp xếp đầu tiên cuối cùng"
description: "Ta có hoán vị các số nguyên từ 1 đến n. Một thao tác lấy bất kỳ phần tử nào và di chuyển nó về phía trước hoặc về phía sau. Thứ tự tương đối của mọi phần tử khác không thay đổi."
date: "2026-08-16T19:38:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 75
verified: true
draft: false
---

[CF 102277H - Sắp xếp cuối cùng đầu tiên](https://codeforces.com/problemset/problem/102277/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có hoán vị các số nguyên từ 1 đến n. Một thao tác lấy bất kỳ phần tử nào và di chuyển nó về phía trước hoặc về phía sau. Thứ tự tương đối của mọi phần tử khác không thay đổi. 

Mục đích là sắp xếp hoán vị thành 

1,2,3,…,n 

sử dụng càng ít thao tác như vậy càng tốt. Đầu vào chứa n, theo sau là hoán vị, một giá trị trên mỗi dòng. Đầu ra là số lần di chuyển đầu tiên hoặc cuối cùng tối thiểu cần thiết. 

Hạn chế quan trọng là mọi giá trị đều khác biệt và mọi giá trị từ 1 đến n xảy ra chính xác một lần. Giới hạn chính thức là n 10 5, với giới hạn thời gian 1 giây và bộ nhớ 256 MB. Một thuật toán bậc hai sẽ thực hiện khoảng 10 10 lần lặp khi n=10 5, vượt xa những gì phù hợp trong một giây. Chúng ta cần một giải pháp tuyến tính cơ bản. 

Các hoạt động có tác dụng rất cụ thể. Nếu một phần tử được di chuyển, nó chỉ có thể trở thành một phần tiền tố hoặc hậu tố của mảng được sắp xếp cuối cùng. Nó không thể được chèn vào giữa. Điều này có nghĩa là câu hỏi thú vị không thực sự là chúng ta di chuyển phần tử nào, mà là khối giá trị liên tiếp nào chúng ta có thể giữ nguyên. 

Hãy xem xét hoán vị```
521354
```Các giá trị 1,2,3 không theo thứ tự vị trí tăng dần vì 2 xuất hiện trước 1, nhưng 3,4,5 lại theo thứ tự bắt buộc. Chúng ta có thể giữ nguyên 3,4,5 và di chuyển 1,2, vì vậy câu trả lời là 2. Một cách tiếp cận bất cẩn chỉ đơn giản là tìm ra dãy con tăng dài nhất sẽ xem xét 1,3,4,5, có độ dài bằng bốn và trả lời sai 1. Các giá trị đó đang tăng về vị trí, nhưng chúng không liên tiếp về giá trị, vì vậy không có cách nào để chèn số 2 còn thiếu vào giữa 1 và 3 chỉ bằng cách di chuyển trước và sau. 

Một trường hợp cạnh khác là hoán vị đã được sắp xếp.```
41234
```Câu trả lời là 0. Bất kỳ triển khai nào khởi tạo câu trả lời của nó cho ít nhất một hoặc chỉ tìm kiếm các lần chạy chứa nhiều hơn một phần tử, đều có thể thất bại ở đây. 

Hoán vị ngược lại là một trường hợp ranh giới hữu ích khác.```
44321
```Chỉ một giá trị có thể thuộc về một khối liên tiếp chưa được chạm tới, vì vậy câu trả lời là 3. Phép tính chuỗi con tăng dài nhất cũng cho một giá trị ở đây, nhưng vì một lý do khác, có thể che giấu cấu trúc thực sự của bài toán. 

Câu lệnh yêu cầu tất cả các giá trị phải khác biệt, do đó, đầu vào trong đó mọi giá trị đều bằng nhau không phải là trường hợp kiểm thử hợp lệ. Tương tự có liên quan để kiểm tra việc triển khai là một hoán vị trong đó mọi mối quan hệ giá trị liền kề bị phá vỡ, chẳng hạn như hoán vị ngược ở trên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi khoảng giá trị có thể có [l,r], kiểm tra xem các giá trị đó đã xuất hiện theo đúng thứ tự tương đối hay chưa và giữ khoảng thời gian hợp lệ dài nhất. Nếu khoảng có độ dài k, chúng ta di chuyển n−k phần tử khác để nó đưa ra câu trả lời là n−k. 

Việc kiểm tra trở nên đặc biệt đơn giản sau khi xây dựng`pos`, Ở đâu`pos[x]`là vị trí tại đó giá trị x xuất hiện. Khoảng [l,r] có thể giữ nguyên chính xác khi 

pos[l]<pos[l+1]<⋯<pos[r]. 

Việc triển khai đơn giản có thể thử mọi giá trị bắt đầu và kéo dài khoảng thời gian cho đến khi điều kiện này không thành công. Trong trường hợp xấu nhất, một hoán vị đã được sắp xếp sẵn sẽ làm cho mọi tiện ích mở rộng thành công, vì vậy điều này thực hiện 

1+2+⋯+n=O(n 2 ) 

séc, tức là khoảng 5⋅10 9 séc cho n=10 5, đã quá lớn. Một lực lượng vũ phu thậm chí còn theo nghĩa đen hơn để kiểm tra mọi khoảng thời gian và quét nội dung của nó sẽ là khối. 

Quan sát quan trọng là tính hợp lệ của một khoảng chỉ phụ thuộc vào các giá trị liền kề. Nếu như`pos[x] < pos[x+1]`, thì x và x+1 có thể là thành viên liên tiếp của khối chưa được chạm tới. Do đó, chúng tôi quét các giá trị từ 1 đến n, duy trì độ dài hiện tại của các giá trị liên tiếp có vị trí tăng dần. 

Ví dụ: nếu mảng vị trí là```
value:  1  2  3  4  5  6pos:    5  6  2  3  4  1
```thì sự so sánh là```
pos[1] < pos[2]   yespos[2] < pos[3]   nopos[3] < pos[4]   yespos[4] < pos[5]   yespos[5] < pos[6]   no
```vì vậy khối liên tiếp hợp lệ dài nhất có độ dài 3, cụ thể là 3,4,5. 

Phương pháp brute-force hoạt động vì một khối nguyên vẹn sẽ xác định hoàn toàn phần tử nào có thể giữ nguyên vị trí. Nó thất bại vì nó liên tục kiểm tra các khoảng thời gian chồng chéo. Quan sát cho thấy chỉ các giá trị liền kề mới quan trọng cho phép chúng ta tìm khối hợp lệ dài nhất trong một lần truyền. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 ) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị và xây dựng`pos`, Ở đâu`pos[x]`là chỉ số của giá trị x. Chúng tôi sử dụng vị trí vì thứ tự sắp xếp cuối cùng được xác định bởi các giá trị số, trong khi đầu vào cung cấp cho chúng tôi vị trí hiện tại của chúng. 
2. Bắt đầu`current = 1`Và`best = 1`. Một giá trị duy nhất luôn có thể được giữ nguyên, vì vậy khối hợp lệ dài nhất có độ dài ít nhất một. 
3. Quét x từ 2 đến n. Nếu như`pos[x - 1] < pos[x]`, mở rộng khối hiện tại bằng cách thiết lập`current += 1`. Điều này có nghĩa là x−1 và x đã xuất hiện theo cùng một thứ tự tương đối được yêu cầu bởi hoán vị được sắp xếp. 
4. Nếu`pos[x - 1] >= pos[x]`, cài lại`current`đến 1. Các giá trị x−1 và x không thể cùng thuộc về cùng một khối liên tiếp chưa được chạm tới, vì vậy bất kỳ khối nào kết thúc tại x−1 đều phải dừng ở đó. 
5. Cập nhật`best = max(best, current)`sau mỗi lần so sánh. Cuối cùng,`best`là số lượng giá trị liên tiếp tối đa có thể không bị ảnh hưởng. 
6. Đầu ra`n - best`. Mọi giá trị bên ngoài khối được giữ lại phải được di chuyển và mọi giá trị bên trong khối có thể giữ nguyên vị trí của nó. 

### Tại sao nó hoạt động 

Giả sử một tập hợp chưa được chạm chứa hai giá trị liên tiếp x và x+1. Vì cả hai giá trị đều không được di chuyển nên thứ tự tương đối của chúng không bao giờ thay đổi. Trong mảng được sắp xếp, x phải xuất hiện trước x+1, vì vậy nhất thiết phải có`pos[x] < pos[x+1]`. 

Mạnh mẽ hơn, tất cả các giá trị chưa được chạm tới phải tạo thành một khoảng giá trị liên tiếp. Nếu chúng ta giữ nguyên x và x+2 mà di chuyển x+1, thì giá trị được di chuyển chỉ có thể được đặt ở phía trước hoặc phía sau, do đó nó không bao giờ có thể nằm giữa x và x+2. Do đó, phần không bị ảnh hưởng có dạng l,l+1,…,r và nó đúng khi vị trí của chúng tăng nghiêm ngặt. 

Đối với bất kỳ khoảng thời gian hợp lệ nào như vậy, hãy di chuyển mọi giá trị nhỏ hơn lên phía trước theo thứ tự giảm dần. Mỗi lần di chuyển sẽ đặt chính xác giá trị tiền tố bắt buộc tiếp theo. Sau đó di chuyển mọi giá trị lớn hơn về phía sau theo thứ tự tăng dần. Khoảng cách không bị ảnh hưởng nằm giữa hai phần này, tạo ra hoán vị được sắp xếp hoàn chỉnh. Do đó nếu khoảng thời gian hợp lệ dài nhất có độ dài`best`, chính xác n− bước di chuyển tốt nhất là đủ và không có giải pháp nào có thể sử dụng ít bước di chuyển hơn vì mọi phần tử bên ngoài một khoảng như vậy đều phải được di chuyển. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    pos = [0] * (n + 1)
    for i in range(1, n + 1):        x = int(input())        pos[x] = i
    best = 1    current = 1
    for x in range(2, n + 1):        if pos[x - 1] < pos[x]:            current += 1        else:            current = 1
        if current > best:            best = current
    print(n - best)

if __name__ == "__main__":    solve()
```Vòng lặp đầu tiên xây dựng hoán vị nghịch đảo. Nếu đầu vào là`2 1 3 5 4`, sau đó`pos[1] = 2`,`pos[2] = 1`,`pos[3] = 3`,`pos[4] = 5`, Và`pos[5] = 4`. 

Vòng lặp thứ hai thực hiện quét vị trí liên tiếp từ thuật toán. Việc so sánh phải giữa`pos[x - 1]`Và`pos[x]`, không phải giữa các phần tử liền kề của hoán vị ban đầu. Chúng tôi đang hỏi liệu các giá trị số liên tiếp đã xuất hiện theo đúng thứ tự hay chưa. 

Khi so sánh thành công, khối liên tiếp hiện tại sẽ tăng lên. Khi thất bại, khối phải khởi động lại ở giá trị hiện tại. Bản cập nhật của`best`được thực hiện sau một trong hai trường hợp, do đó khối kết thúc ở giá trị cuối cùng n được xử lý chính xác. 

Không có vấn đề tràn số nguyên trong Python và các mảng chỉ chứa n+1 số nguyên. Việc triển khai cũng xử lý n=1:`best`bắt đầu từ một, quá trình quét trống và câu trả lời là`1 - 1 = 0`. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên từ vấn đề là:```
883674152
```Hoán vị nghịch đảo của nó được hiển thị dưới đây. 

| Giá trị`x`|`pos[x]`| So sánh |`current`|`best`| 
| --- | --- | --- | --- | --- | 
| 1 | 6 | bắt đầu | 1 | 1 | 
| 2 | 8 |`6 < 8`| 2 | 2 | 
| 3 | 2 |`8 < 2`sai | 1 | 2 | 
| 4 | 5 |`2 < 5`| 2 | 2 | 
| 5 | 7 |`5 < 7`| 3 | 3 | 
| 6 | 3 |`7 < 3`sai | 1 | 3 | 
| 7 | 4 |`3 < 4`| 2 | 3 | 
| 8 | 1 |`4 < 1`sai | 1 | 3 | 

Khối liên tiếp dài nhất chưa được chạm tới có chiều dài ba, cụ thể là 4,5,6. Năm giá trị còn lại có thể được chuyển đến đầu thích hợp, vì vậy câu trả lời là`8 - 3 = 5`. 

Một ví dụ thứ hai là:```
521354
```Quá trình quét là: 

| Giá trị`x`|`pos[x]`| So sánh |`current`|`best`| 
| --- | --- | --- | --- | --- | 
| 1 | 2 | bắt đầu | 1 | 1 | 
| 2 | 1 |`2 < 1`sai | 1 | 1 | 
| 3 | 3 |`1 < 3`| 2 | 2 | 
| 4 | 5 |`3 < 5`| 3 | 3 | 
| 5 | 4 |`5 < 4`sai | 1 | 3 | 

Khối hợp lệ dài nhất là 2,3,4, xuất hiện ở vị trí 1,3,5. Các giá trị 1 và 5 có thể được di chuyển tương ứng về phía trước và phía sau, tạo ra mảng được sắp xếp theo hai thao tác. Câu trả lời là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng`pos`mất O(n) và lần quét liên tiếp cũng mất O(n). | 
| Không gian | O(n) | Hoán vị nghịch đảo yêu cầu n+1 số nguyên. | 

Với n 10 5, thuật toán chỉ thực hiện một số thao tác không đổi trên mỗi giá trị đầu vào. Điều đó thoải mái phù hợp với giới hạn 1 giây và 256 MB được chỉ định cho sự cố. 

## Trường hợp thử nghiệm 

Vấn đề ban đầu cung cấp một mẫu, được bao gồm bên dưới. Vì câu lệnh yêu cầu hoán vị nên phép kiểm tra hoàn toàn bằng nhau không hợp lệ và được thay thế bằng hoán vị ngược, điều này nhấn mạnh trường hợp mọi so sánh giá trị liên tiếp liền kề đều thất bại.```python
Pythonimport sysimport io

def solve_data(inp: str) -> str:    data = list(map(int, inp.split()))    it = iter(data)    n = next(it)
    pos = [0] * (n + 1)    for i in range(1, n + 1):        x = next(it)        pos[x] = i
    best = 1    current = 1
    for x in range(2, n + 1):        if pos[x - 1] < pos[x]:            current += 1        else:            current = 1        best = max(best, current)
    return str(n - best) + "\n"

# Sample 1assert solve_data(    """8836741
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Đầu vào kích thước tối thiểu và quét trống. | 
|`1 2 3 4 5`|`0`| Đã sắp xếp hoán vị và khối giữ lại tối đa có thể. | 
|`5 4 3 2 1`|`4`| Mọi so sánh vị trí liền kề đều thất bại. | 
|`2 1 3 5 4`|`2`| Một khối liên tiếp hợp lệ xuất hiện ở giữa phạm vi giá trị. | 
|`6 1 2 3 4 5`|`1`| Xử lý đúng khi khối tốt nhất đạt giá trị cuối cùng. | 

## Vỏ cạnh 

Đối với đầu vào kích thước tối thiểu```
11
```không có cặp giá trị liên tiếp nào để so sánh. Thuật toán bắt đầu với`best = current = 1`, do đó kết quả ngay lập tức là 1−1=0. Giá trị duy nhất đã được sắp xếp và không cần thao tác. 

Đối với hoán vị đã được sắp xếp```
512345
```mảng vị trí là`[1,2,3,4,5]`khi được lập chỉ mục theo giá trị. Mọi so sánh đều thành công, vì vậy`current`phát triển từ một đến năm. Thuật toán thu được`best = 5`và đầu ra`0`. Điều này cũng giải thích tại sao quá trình quét phải cập nhật độ dài tốt nhất ngay cả khi khối dài nhất đạt tới n. 

Đối với hoán vị ngược```
554321
```mảng vị trí là`[5,4,3,2,1]`. Mọi sự so sánh`pos[x-1] < pos[x]`là sai, vì vậy`current`được thiết lập lại nhiều lần về một. Như vậy`best = 1`và câu trả lời là 5−1=4. Không có hai giá trị liên tiếp nào có thể giữ nguyên. 

Đối với hoán vị```
521354
```chúng tôi nhận được`pos[1]=2`,`pos[2]=1`,`pos[3]=3`,`pos[4]=5`, Và`pos[5]=4`. So sánh giữa 1 và 2 không thành công, sau đó so sánh giữa 2,3 và 3,4 thành công. Khối hợp lệ dài nhất là 2,3,4, có độ dài ba. Di chuyển 1 về phía trước và 5 về phía sau sẽ sắp xếp hoán vị, do đó kết quả là`2`. 

Sự khác biệt giữa dãy con tăng dài nhất và khối tăng liên tiếp dài nhất là bẫy chính. Ví dụ,```
41324
```chứa dãy con tăng dần 1,3,4, nhưng những giá trị đó không thể giữ nguyên. Nếu 2 được di chuyển, nó chỉ có thể đi về phía trước hoặc phía sau, không bao giờ được chuyển từ 1 đến 3. Các khối không bị chạm hợp lệ là 1, 2,3 và 4, vì vậy`best = 2`và câu trả lời đúng là`2`. Thuật toán nắm bắt được điều này vì nó chỉ mở rộng một khối khi cả giá trị và vị trí đều tăng liên tiếp.
