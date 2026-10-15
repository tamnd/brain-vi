---
title: "CF 105445C - Bài toán Sigma (Phiên bản dễ)"
description: "Chúng ta được cung cấp một mảng duy nhất và được yêu cầu tính tổng lồng nhau rất lớn được xây dựng từ tất cả các mảng con liền kề. Với mỗi chỉ số đầu i, ta xét mọi chỉ số cuối j ≥ i, tính tích của mảng con a[i] ..."
date: "2026-06-23T03:25:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105445
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #36 (Starters-Forces)"
rating: 0
weight: 105445
solve_time_s: 90
verified: false
draft: false
---

[CF 105445C - Sự cố Sigma (Phiên bản dễ)](https://codeforces.com/problemset/problem/105445/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng duy nhất và được yêu cầu tính tổng lồng nhau rất lớn được xây dựng từ tất cả các mảng con liền kề. Đối với mọi chỉ số bắt đầu`i`, chúng tôi xem xét mọi chỉ số kết thúc`j ≥ i`, tính tích của mảng con`a[i] ... a[j]`, rồi tính tổng tất cả các giá trị này trên tất cả các giá trị có thể`(i, j)`cặp. 

Vì vậy, số lượng về cơ bản là tổng đóng góp của mỗi mảng con, trong đó mỗi mảng con đóng góp sản phẩm của nó một lần. 

Ngay cả trong phiên bản dễ dàng này, đầu vào có thể chứa tới một triệu phần tử trong các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào liệt kê rõ ràng các mảng con hoặc tính toán lại sản phẩm từ đầu sẽ thất bại ngay lập tức. Một số bậc hai của mảng con đã cho biết`5e11`hoạt động ở kích thước tối đa và mỗi phép tính sản phẩm sẽ thêm một yếu tố tuyến tính khác nếu không được sử dụng lại cẩn thận. 

Khó khăn chính là sản phẩm tăng theo cấp số nhân, nhưng tổng yêu cầu là cộng trên tất cả các mảng con, vì vậy chúng ta cần một cách để sử dụng lại cấu trúc từng phần trên các mảng con chồng chéo. 

Một trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị được`1`. Trong trường hợp đó mọi tích của mảng con đều là`1`, và câu trả lời sẽ trở thành tổng số mảng con, tức là`n(n+1)/2`được tổng hợp lại theo điểm bắt đầu, tạo ra mô hình tăng trưởng dạng bậc ba theo cách suy luận ngây thơ. Đây chính xác là nơi mô phỏng lực lượng vũ phu sụp đổ. 

Một trường hợp khác là các giá trị lớn gần bằng`1e9`. Phép nhân trực tiếp mà không rút gọn mô-đun sẽ làm tràn số nguyên 64 bit nếu chúng ta không cẩn thận và ngay cả số học modulo cũng yêu cầu thứ tự cẩn thận để tránh chi phí tính toán lại. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi chỉ số bắt đầu`i`, chúng tôi mở rộng`j`từ`i`ĐẾN`n`, duy trì một sản phẩm đang hoạt động của`a[i]...a[j]`, và tích lũy nó vào câu trả lời. Điều này khớp chính xác với định nghĩa và hoạt động vì mỗi mảng con được truy cập chính xác một lần. 

Tuy nhiên, cách tiếp cận này tính toán lại hoặc cập nhật liên tục các sản phẩm cho mỗi`(i, j)`cặp, dẫn đến`O(n^2)`phép nhân cho mỗi trường hợp thử nghiệm. Với tổng số`n`lên tới`10^6`, điều này trở nên quá chậm. 

Quan sát quan trọng là chúng ta đang tính tổng các đóng góp của tất cả các mảng con và mỗi phần tử tham gia vào nhiều mảng con theo cách có cấu trúc. Thay vì suy nghĩ theo các mảng con, chúng ta có thể đảo ngược quan điểm và coi sự đóng góp của từng vị trí như một phần của sự tích lũy động đối với các tiền tố. 

Nếu chúng ta sửa điểm cuối`j`, mọi mảng con kết thúc tại`j`có thể được coi là các mảng con mở rộng kết thúc tại`j-1`bằng cách nhân với`a[j]`, sau đó thêm mảng con một phần tử`[j]`. Điều này gợi ý việc duy trì hai đại lượng luân phiên: tổng của tất cả các tích của mảng con kết thúc tại`j`, và câu trả lời toàn cầu. 

Điều này biến vấn đề thành một phép lặp tuyến tính trong đó mỗi bước cập nhật tất cả các đóng góp trước đó theo hệ số`a[j]`, nhưng thay vì cập nhật rõ ràng tất cả các trạng thái trước đó, chúng tôi sử dụng lại thực tế là tổng của tất cả các mảng con kết thúc tại`j-1`đã mã hóa sản phẩm của họ. Nhân số tiền đó với`a[j]`tự động chuyển đổi mọi mảng con kết thúc tại`j-1`vào phiên bản mở rộng của nó kết thúc tại`j`. 

Vì vậy quá trình chuyển đổi trở thành thời gian cục bộ và không đổi cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| DP tối ưu qua đóng góp hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì giá trị cuộn biểu thị tổng của tất cả các tích của mảng con kết thúc ở chỉ mục hiện tại. 

1. Khởi tạo một biến`end_sum`để lưu trữ tổng tích của tất cả các mảng con kết thúc ở vị trí hiện tại. Khởi tạo`answer`để tích lũy kết quả cuối cùng. 
2. Lặp lại mảng từ chỉ mục`0`ĐẾN`n-1`. 
3. Tại vị trí`j`, mọi mảng con kết thúc tại`j`là một mảng con mới chỉ bao gồm`a[j]`hoặc phần mở rộng của mảng con kết thúc tại`j-1`nhân với`a[j]`. Điều này gây ra sự tái phát`end_sum = a[j] + end_sum * a[j] (mod M)`. 

Lý do điều này có hiệu quả là vì việc nhân tất cả các mảng con kết thúc trước đó với`a[j]`mở rộng chúng một cách chính xác mà không làm mất bất kỳ cấu trúc nào. 
4. Thêm`end_sum`vào toàn cầu`answer`. Điều này nắm bắt tất cả các mảng con kết thúc tại`j`. 
5. Tiếp tục cho đến hết mảng. trận chung kết`answer`là tổng của tất cả các mảng con. 

### Tại sao nó hoạt động 

Tại mọi chỉ số`j`,`end_sum`chính xác bằng tổng tích của tất cả các mảng con có điểm cuối bên phải là`j`. Điều này được bảo toàn theo cách quy nạp bởi vì mọi mảng con như vậy đều bắt đầu tại`j`hoặc được hình thành bằng cách mở rộng một mảng con kết thúc tại`j-1`. Vì phép nhân phân phối cho phép cộng nên việc mở rộng tất cả các mảng con trước đó bằng`a[j]`tương đương với việc nhân tổng số tiền đóng góp của họ với`a[j]`và việc thêm mảng con singleton sẽ hoàn thành tập hợp. Bởi vì mỗi mảng con có chính xác một điểm cuối bên phải, tính tổng tất cả`end_sum`giá trị bao gồm mọi mảng con chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        end_sum = 0
        ans = 0
        
        for x in a:
            end_sum = (x + end_sum * x) % MOD
            ans = (ans + end_sum) % MOD
        
        # q = 1 and l=1, r=n always
        input()  # consume query line
        print(ans)

if __name__ == "__main__":
    solve()
```Trạng thái quan trọng trong việc thực hiện là`end_sum`, đại diện cho tất cả các mảng con kết thúc ở chỉ mục hiện tại. Mỗi bản cập nhật sẽ nhân trạng thái trước đó với phần tử hiện tại, sau đó thêm mảng con một phần tử mới. Bộ tích lũy thứ hai`ans`tổng hợp các đóng góp trên tất cả các điểm cuối. 

Đầu vào luôn cung cấp một truy vấn duy nhất bao trùm toàn bộ mảng, vì vậy chúng tôi chỉ cần tính kết quả chung một lần cho mỗi trường hợp kiểm thử và bỏ qua giới hạn truy vấn sau khi đọc chúng. 

Một sai lầm phổ biến là quên rằng phép truy toán áp dụng modulo ở mỗi bước. Nếu không giảm mô-đun sau khi nhân, các giá trị trung gian sẽ tăng quá lớn ngay cả đối với những cân nhắc về hiệu suất của Python trong ngữ cảnh được biên dịch. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [2, 4, 3]
```Chúng tôi theo dõi`end_sum`Và`ans`. 

| j | một[j] | end_sum trước | end_sum sau | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 2 | 2 | 
| 1 | 4 | 2 | 4 + 2*4 = 12 | 14 | 
| 2 | 3 | 12 | 3 + 12*3 = 39 | 53 | 

Kết quả cuối cùng`53`khớp với tổng của tất cả các sản phẩm mảng con. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 1, 1, 1]
```| j | một[j] | end_sum trước | end_sum sau | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 
| 1 | 1 | 1 | 2 | 3 | 
| 2 | 1 | 2 | 3 | 6 | 
| 3 | 1 | 3 | 4 | 10 | 

Điều này tạo ra tổng số mảng con của tất cả các hậu tố được tích lũy, xác nhận phép truy toán hoạt động chính xác dưới các giá trị thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến đang chạy được duy trì | 

Các ràng buộc cho phép tổng cộng tối đa một triệu phần tử, do đó quét tuyến tính cho mỗi trường hợp thử nghiệm là tối ưu. Giải pháp chỉ thực hiện một số phép tính số học không đổi cho mỗi phần tử, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve_input(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    MOD = 10**9 + 7

    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        end_sum = 0
        ans = 0
        for x in a:
            end_sum = (x + end_sum * x) % MOD
            ans = (ans + end_sum) % MOD

        input()
        print(ans)

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert solve_input("""1
3 1
2 4 3
1 3
""") == "53"

# custom case: minimum size
assert solve_input("""1
2 1
5 7
1 2
""") == str((5 + 7 + 5*7) % MOD)

# all ones
assert solve_input("""1
5 1
1 1 1 1 1
1 5
""") == "15"

# increasing pattern
assert solve_input("""1
3 1
1 2 3
1 3
""") == str((1 + 2 + 3 + 2 + 6 + 6) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng nhỏ cỡ 2 | kết quả liệt kê trực tiếp | cơ sở đúng đắn của phép truy hồi | 
| tất cả những cái | tăng trưởng mảng con tam giác | hành vi mở rộng lặp đi lặp lại | 
| trình tự tăng dần | sản phẩm hỗn hợp | tính đúng đắn trong điều kiện tăng trưởng không đồng đều | 

## Vỏ cạnh 

Đối với một mảng của tất cả những cái đó, hãy nói`a = [1, 1, 1]`, sự tái phát mang lại`end_sum`giá trị`1, 2, 3`, và đáp án cuối cùng`6`. Điều này phù hợp với thực tế là có chính xác sáu mảng con, mỗi mảng có giá trị đóng góp`1`. Thuật toán tích lũy chúng một cách chính xác vì mọi tiện ích mở rộng đều bảo toàn giá trị. 

Đối với một phần tử lớn như`a = [10^9]`, sự tái phát tạo ra`end_sum = 10^9`và trả lời`10^9`. Vì chỉ có một mảng con nên không xảy ra tương tác tràn hoặc ẩn và số học modulo trực tiếp ghi lại kết quả. 

Đối với một mảng tăng nghiêm ngặt như`[1, 2, 3, 4]`, mỗi bước sẽ nhân tất cả các đóng góp của mảng con trước đó với phần tử mới và thêm phần tử đơn mới. Việc theo dõi cho thấy rằng không có phân mảng nào được tính hai lần vì mỗi mảng được neo duy nhất ở điểm cuối bên phải của nó, đảm bảo tính toán rời rạc qua các lần lặp.
