---
title: "CF 102788K - Tháp Hà Nội"
description: "Bài toán đưa ra vị trí trung gian của một số đội khi họ thực hiện giải pháp Tháp Hà Nội ba thanh cổ điển. Mỗi đội tuân theo chính xác quy trình đệ quy giống nhau, di chuyển tất cả N đĩa từ thanh A sang thanh B."
date: "2026-08-03T15:08:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102788
codeforces_index: "K"
codeforces_contest_name: "2017-2018 ICPC Central Quarter Final of Northeastern European Regional Collegiate Programming Contest"
rating: 0
weight: 102788
solve_time_s: 59
verified: true
draft: false
---

[CF 102788K - Tháp Hà Nội](https://codeforces.com/problemset/problem/102788/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra vị trí trung gian của một số đội khi họ thực hiện giải pháp Tháp Hà Nội ba thanh cổ điển. Mỗi đội tuân theo chính xác cùng một quy trình đệ quy, di chuyển tất cả`N`đĩa từ thanh`A`đánh gậy`B`. Đối với mỗi đội, chúng tôi được cung cấp một chuỗi có độ dài`N`, trong đó ký tự ở vị trí`i`mô tả thanh hiện tại của đĩa`i`, với đĩa`1`là nhỏ nhất và đĩa`N`là lớn nhất. Nhiệm vụ là xác định xem nhóm nào tiến xa nhất trong quy trình, nghĩa là nhóm có cấu hình được ghi lại xuất hiện muộn nhất trong trình tự tối ưu duy nhất. Nếu nhiều đội có cùng đội hình thì phải chọn đội đầu tiên trong số đó. 

Số lượng đĩa có thể đạt tới`1000`, và số lượng đội cũng có thể đạt tới`1000`. Tổng số lần di chuyển là`2^N - 1`, do đó việc lưu trữ hoặc mô phỏng toàn bộ chuỗi là không thể ngay cả đối với kích thước nhỏ vừa phải`N`. Một mô phỏng trực tiếp đã trở nên vô dụng`N = 60`, bởi vì số lần di chuyển vượt quá những gì có thể được xử lý trong giới hạn thời gian thông thường. Giải pháp phải hoạt động trong khoảng`O(NM)`thời gian, vì bản thân đầu vào chứa`NM`nhân vật. 

Trường hợp khó khăn đầu tiên là số lần di chuyển không được lưu trữ trực tiếp và không thể khớp với các loại số nguyên tiêu chuẩn. Ví dụ, với`N = 1000`, câu trả lời có thể cần khoảng một nghìn bit. Giải pháp chuyển đổi cấu hình thành số nguyên bình thường sẽ tràn hoặc tốn thời gian không cần thiết cho số học khổng lồ. 

Một trường hợp khác là khi một số đội có trạng thái giống hệt nhau. Ví dụ:```
2 3
AA
BB
BB
```Đầu ra đúng là:```
2
```Hai đội cuối cùng đều hoàn thành câu đố nhưng phải chọn đội xuất hiện đầu tiên. Việc thực hiện bất cẩn làm cập nhật câu trả lời trên`>=`thay vì chỉ bật`>`sẽ trả lại đội không chính xác`3`. 

Trường hợp cạnh thứ hai là cấu hình ban đầu:```
3 1
AAA
```Đầu ra đúng là:```
1
```Trạng thái đầu tiên tương ứng với việc di chuyển`0`. Việc triển khai giả định rằng ít nhất một đĩa đã được di chuyển có thể phân loại sai cấu hình này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra mọi cấu hình của quy trình Hà Nội và so sánh từng trạng thái được tạo với trạng thái của các đội. Quy trình đệ quy rất dễ mô phỏng vì mỗi bước di chuyển đều được biết trước. Vấn đề là trình tự có`2^N`tiểu bang. Vì`N = 1000`, điều này vượt xa mọi giới hạn có thể có. Ngay cả việc tạo ra một chuyển động trong mỗi nano giây cũng không làm cho phương pháp này trở nên thực tế. 

Nhận xét quan trọng là dãy Hà Nội cổ điển có cấu trúc đệ quy. Khi giải quyết`N`đĩa từ`A`ĐẾN`B`, nửa đầu của nước đi được giải quyết`N-1`đĩa từ`A`ĐẾN`C`, sau đó là đĩa`N`di chuyển từ`A`ĐẾN`B`, thì phần còn lại`N-1`đĩa di chuyển từ`C`ĐẾN`B`. Điều này có nghĩa là vị trí của đĩa lớn nhất ngay lập tức cho chúng ta biết nửa nào của chuỗi chứa trạng thái hiện tại. 

Nếu đĩa lớn nhất vẫn còn trên thanh nguồn thì câu trả lời nằm ở đâu đó trong nửa đầu. Nếu nó nằm trên thanh đích, câu trả lời nằm ở nửa sau và chúng ta chỉ cần tiếp tục đệ quy trên các đĩa nhỏ hơn với vai trò thanh được hoán đổi. Thay vì xây dựng toàn bộ chuỗi, chúng ta xây dựng lại biểu diễn nhị phân của số nước đi. Mỗi đĩa đóng góp một bit: liệu thời điểm đĩa đó di chuyển đã xảy ra hay chưa. 

Phương pháp brute-force hoạt động vì trình tự Hà Nội có tính xác định, nhưng thất bại vì trình tự tăng theo cấp số nhân. Cấu trúc đệ quy cho phép chúng ta giảm vấn đề xuống còn việc kiểm tra từng đĩa một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^N)`|`O(N)`| Quá chậm | 
| Tối ưu |`O(NM)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cấu hình mỗi đội bắt đầu với đĩa lớn nhất và thông số gốc Hà Nội: di chuyển từ que`A`đánh gậy`B`sử dụng que`C`. 
2. Nhìn vào vị trí của đĩa lớn nhất hiện tại. Nếu nó vẫn còn trên thanh nguồn, số bước di chuyển thuộc nửa đầu của chuỗi đệ quy, do đó bit hiện tại là`0`. Bài toán đệ quy tiếp theo là di chuyển các đĩa còn lại từ cùng một nguồn sang thanh phụ. 
3. Nếu đĩa lớn nhất nằm trên thanh đích thì số lần di chuyển sẽ thuộc về nửa sau. Bit hiện tại là`1`, và các đĩa còn lại phải được xem xét trong giai đoạn đệ quy thứ hai, di chuyển từ thanh phụ đến thanh đích. 
4. Tiếp tục cho đến khi tất cả các đĩa đã được xử lý. Chuỗi bit mô tả số lần di chuyển. Thay vì xây dựng số nguyên khổng lồ, hãy so sánh các chuỗi bit này từ phía quan trọng nhất. 
5. Giữ cho nhóm có biểu diễn nước đi nhị phân lớn nhất. Chỉ thay thế câu trả lời hiện tại khi tìm thấy giá trị lớn hơn, giá trị này sẽ tự động bảo toàn đội đầu tiên trong trường hợp hòa. 

Lý do điều này có tác dụng là đĩa lớn nhất thay đổi vị trí chính xác một lần trong toàn bộ giải pháp tối ưu. Trước động thái đó, mọi cấu hình phải thuộc khối đệ quy đầu tiên. Sau bước di chuyển đó, mọi cấu hình đều thuộc về khối đệ quy thứ hai. Việc lặp lại đối số này cho các đĩa nhỏ hơn sẽ xác định duy nhất vị trí của mọi trạng thái trong chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_bits(state):
    n = len(state)
    src, dst, aux = 'A', 'B', 'C'
    bits = []

    for disk in range(n - 1, -1, -1):
        pos = state[disk]
        if pos == src:
            bits.append('0')
            src, dst, aux = src, aux, dst
        else:
            bits.append('1')
            src, dst, aux = aux, dst, src

    return ''.join(bits)

def solve():
    n, m = map(int, input().split())

    best_team = 1
    best = None

    for i in range(1, m + 1):
        state = input().strip()
        value = get_bits(state)

        if best is None or value > best:
            best = value
            best_team = i

    print(best_team)

if __name__ == "__main__":
    solve()
```chức năng`get_bits`thực hiện lý luận đệ quy mà không đệ quy. Vòng lặp bắt đầu với đĩa lớn nhất vì nó xác định bit cao nhất của số di chuyển. Việc xử lý các đĩa từ lớn nhất đến nhỏ nhất tương đương với việc giảm dần các lệnh gọi đệ quy. 

Khi đĩa hiện tại nằm trên thanh nguồn, việc di chuyển vẫn chưa xảy ra nên bit tương ứng là`0`. Các đĩa nhỏ hơn vẫn đang ở giai đoạn đệ quy đầu tiên, giai đoạn này sẽ thay đổi thanh phụ thành đích tạm thời. Khi đĩa nằm trên thanh đích, việc di chuyển đã xảy ra, do đó bit được`1`và vai trò của các thanh thay đổi để mô tả pha thứ hai. 

Việc so sánh được thực hiện trên các chuỗi vì mọi trạng thái đều tạo ra chính xác`N`bit. Các chuỗi nhị phân có độ dài bằng nhau so sánh chính xác về mặt từ điển với`1`lớn hơn`0`, do đó không cần chuyển đổi số nguyên lớn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 7
CAAA
AAAA
CCCB
CBAA
BBAA
BBCA
CCCA
```Các biểu diễn di chuyển được tạo ra là: 

| Đội | Tiểu bang | Biểu diễn di chuyển nhị phân | 
| --- | --- | --- | 
| 1 | CAAA | 0010 | 
| 2 | AAAA | 0000 | 
| 3 | CCCB | 1100 | 
| 4 | CBAA | 0011 | 
| 5 | BBAA | 0101 | 
| 6 | BBCA | 0110 | 
| 7 | CCCA | 1010 | 

Giá trị nhị phân lớn nhất là`1100`, thuộc về đội`3`. 

Đối với mẫu thứ hai:```
3 4
AAA
BBB
BAA
BBB
```| Đội | Tiểu bang | Biểu diễn di chuyển nhị phân | 
| --- | --- | --- | 
| 1 | AAA | 000 | 
| 2 | BBB | 111 | 
| 3 | BAA | 001 | 
| 4 | BBB | 111 | 

Đội`2`Và`4`cả hai đều đang ở bước cuối cùng. Vì hòa giữ đội trước nên câu trả lời là đội`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NM)`| Chuỗi của mỗi đội được quét một lần, kiểm tra từng đĩa chính xác một lần. | 
| Không gian |`O(N)`| Biểu diễn bit tạm thời có độ dài`N`. | 

Với`N`Và`M`nhiều nhất là cả hai`1000`, điều này thực hiện khoảng một triệu thao tác đơn giản và dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def get_bits(state):
        src, dst, aux = 'A', 'B', 'C'
        bits = []

        for disk in range(len(state) - 1, -1, -1):
            if state[disk] == src:
                bits.append('0')
                src, dst, aux = src, aux, dst
            else:
                bits.append('1')
                src, dst, aux = aux, dst, src

        return ''.join(bits)

    n, m = map(int, input().split())
    ans = 1
    best = None

    for i in range(1, m + 1):
        cur = get_bits(input().strip())
        if best is None or cur > best:
            best = cur
            ans = i

    return str(ans)

assert solve_data("""4 7
CAAA
AAAA
CCCB
CBAA
BBAA
BBCA
CCCA
""") == "3"

assert solve_data("""3 4
AAA
BBB
BAA
BBB
""") == "2"

assert solve_data("""1 3
A
B
B
""") == "2"

assert solve_data("""2 3
AA
BB
BB
""") == "2"

assert solve_data("""5 2
AAAAA
BBBBB
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / A / B / B`|`2`| Xử lý đĩa đơn | 
|`2 3 / AA / BB / BB`|`2`| Xử lý cà vạt | 
|`5 2 / AAAAA / BBBBB`|`2`| Số di chuyển lớn mà không cần chuyển đổi số nguyên | 
| Trường hợp mẫu |`3`,`2`| Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với trường hợp trạng thái trùng lặp:```
2 3
AA
BB
BB
```Trạng thái đầu tiên có số di chuyển`0`. Cả hai`BB`tiểu bang có số di chuyển`3`. Thuật toán tạo ra chuỗi bit giống nhau cho các đội`2`Và`3`và bởi vì nó chỉ cập nhật khi giá trị mới lớn hơn rất nhiều nên nhóm`2`vẫn được chọn. 

Đối với cấu hình ban đầu:```
3 1
AAA
```Mọi đĩa vẫn còn trên thanh nguồn. Thuật toán ghi lại ba bit 0, biểu thị số lần di chuyển`0`. Không cần xử lý đặc biệt vì quy tắc đệ quy tương tự được áp dụng ngay cả trước khi bất kỳ động thái nào xảy ra. 

Đối với đầu vào lớn nhất có thể, thuật toán không bao giờ xây dựng chuỗi Hà Nội. Nó chỉ đọc từng`1000`các ký tự cho mỗi`1000`các đội, do đó số lần di chuyển có thể xảy ra theo cấp số nhân không bao giờ xuất hiện trong tính toán.
