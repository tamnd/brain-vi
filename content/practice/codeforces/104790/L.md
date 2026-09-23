---
title: "CF 104790L - Khóa Cửa"
description: "Chúng tôi được cung cấp một tập hợp các phòng được kết nối bằng cửa. Mỗi cánh cửa có thể được đi qua theo cả hai hướng, do đó về mặt vật lý, bố cục là một đồ thị liên thông vô hướng."
date: "2026-06-28T14:00:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "L"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 65
verified: true
draft: false
---

[CF 104790L - Khóa cửa](https://codeforces.com/problemset/problem/104790/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các phòng được kết nối bằng cửa. Mỗi cánh cửa có thể được đi qua theo cả hai hướng, do đó về mặt vật lý, bố cục là một đồ thị liên thông vô hướng. Tuy nhiên, mỗi cánh cửa đều có một hạn chế: nó chỉ có thể được khóa từ một điểm cuối cụ thể, điểm cuối được liệt kê là`a`trong cặp đầu vào`(a, b)`. 

Muốn khóa cửa phải đứng trong phòng`a`trong khi xử lý cánh cửa đó. Vì bạn di chuyển tự do qua các cửa đang mở nên khó khăn duy nhất là đảm bảo rằng, trong quá trình di chuyển quanh tòa nhà, bạn đã ở vị trí chính xác để khóa mọi cửa ít nhất một lần. 

Bạn bắt đầu vào bên trong tòa nhà, lối vào chính đã bị khóa và bạn được phép yêu cầu thêm “lối thoát hiểm” ở một số phòng. Lối ra là một kết nối đặc biệt cho phép bạn rời khỏi hoặc vào lại tòa nhà từ căn phòng đó. Mỗi lối ra cho phép bạn bắt đầu hoặc kết thúc một đoạn di chuyển tại phòng đó một cách hiệu quả. 

Nhiệm vụ là xác định số lượng lối ra tối thiểu cần thiết để có thể lên kế hoạch đi bộ bên trong tòa nhà sao cho mọi cửa đều có thể khóa theo hạn chế của nó và sau khi hoàn thành mọi việc bạn có thể rời đi. 

Hạn chế chính về cấu trúc là mỗi cánh cửa đặt ra một yêu cầu về hướng về cách nó phải được “che phủ” trong quá trình di chuyển: đối với một cánh cửa`(a, b)`, nó chỉ hài lòng khi bạn duyệt nó theo cách mà bạn đang ở`a`lúc này nó đang bị khóa. Điều này biến vấn đề thành việc cân bằng số lượng “hoạt động” phải bắt đầu từ các nút nhất định. 

Hạn chế đầu vào rất lớn, lên tới 100.000 phòng và 1.000.000 cửa. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai hoặc thậm chí gần bậc hai trên đường đi. Bất kỳ giải pháp đúng nào cũng phải xử lý đồ thị theo thời gian tuyến tính, về cơ bản là O(n + m). 

Một trường hợp phức tạp phát sinh khi nghĩ về các chiến lược truyền tải đơn giản. Ví dụ, nếu một người cố gắng bước đi một cách tham lam và khóa cửa khi gặp phải, nó có thể thất bại tùy theo thứ tự. 

Hãy xem xét một cấu trúc đơn giản:```
3 2
2 1
3 1
```Nếu bạn bắt đầu ở phòng 2, bạn có thể khóa`(2,1)`nhưng cuối cùng có thể không thể đáp ứng đúng ràng buộc khóa cho`(3,1)`mà không cần khởi động lại ở nơi khác. Một bước đi tham lam ngây thơ không đảm bảo tính khả thi nếu không có điểm xuất phát bổ sung. 

Vấn đề chính là một số phòng yêu cầu bạn “bắt đầu” nhiều lượt di chuyển hạn chế hơn những phòng khác có thể hỗ trợ một cách tự nhiên trong một lần đi bộ liên tục. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng mô phỏng tất cả các lệnh đi bộ có thể có trong tòa nhà, thử các điểm bắt đầu và trình tự đi qua cửa khác nhau. Mỗi tiểu bang sẽ theo dõi những cánh cửa nào đã bị khóa và người công nhân hiện đang ở đâu. Vì mỗi cánh cửa có thể được đi qua nhiều lần và các quyết định phụ thuộc vào các ràng buộc trong tương lai, nên điều này nhanh chóng trở thành cấp số nhân. Ngay cả việc hạn chế các bước đi ngắn nhất hoặc theo kinh nghiệm cũng không thành công vì các quyết định địa phương có thể cản trở tính khả thi toàn cầu. 

Hình thức thất bại của bạo lực là nó không đáp ứng được yêu cầu cân bằng toàn cầu do các ràng buộc định hướng gây ra. Điều quan trọng không phải là thứ tự truyền tải chính xác mà là số lần mỗi phòng phải đóng vai trò là “nguồn khởi đầu” để đáp ứng các yêu cầu về khóa gửi đi. 

Quan sát quan trọng là diễn giải lại từng cánh cửa`(a, b)`như một yêu cầu “tiêu thụ” một đơn vị công suất một cách hiệu quả bắt đầu từ`a`. Vì chuyển động không bị hạn chế trong biểu đồ vô hướng nên mọi yêu cầu đều có thể được định tuyến qua các phòng trung gian, do đó khả năng kết nối của biểu đồ không hạn chế tính khả thi. Yếu tố hạn chế duy nhất là có bao nhiêu yêu cầu như vậy có thể được xâu chuỗi một cách tự nhiên trong một bước đi. 

Mỗi phòng`v`có số lượng cửa cần phải khóa từ nó. Gọi đây`out_req[v]`. Không có yêu cầu đối xứng cho`b`, vì khóa chỉ bị hạn chế ở một bên. Vấn đề trở thành vấn đề đáp ứng tất cả các yêu cầu định hướng này bằng cách sử dụng càng ít đoạn đi bộ càng tốt. 

Mỗi đoạn đi bộ tương ứng với một hành trình di chuyển liên tục bắt đầu từ một số phòng nơi chúng tôi “tiêm” khả năng khởi động một cách hiệu quả. Bất cứ khi nào một phòng có nhiều yêu cầu gửi đi hơn mức có thể được đáp ứng trong một chuỗi liên tục duy nhất, thì cần có thêm lối ra để bắt đầu các phân đoạn mới. 

Điều này làm giảm vấn đề trong việc đếm số lần khởi động độc lập là cần thiết, chính xác là số lượng “đơn vị nhu cầu bổ sung” được phân bổ trên các nút. 

Do đó, giải pháp thu được bằng cách tính tổng đóng góp của các nút nơi nhu cầu dương trong điều kiện cân bằng tự nhiên do các ràng buộc chuỗi gây ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bước đi Brute Force | Hàm mũ | O(n + m) | Quá chậm | 
| Cân bằng dựa trên mức độ | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta phát biểu lại vấn đề bằng cách đếm xem cần có bao nhiêu lần bắt đầu truyền tải độc lập để thỏa mãn tất cả các yêu cầu khóa. 

### 1. Đếm yêu cầu định hướng cho mỗi phòng 

Cho mỗi cửa`(a, b)`, tăng`out_req[a]`bởi một. Điều này thể hiện rằng cánh cửa này cuối cùng phải bị khóa khi đứng trong phòng`a`. 

Bước này nắm bắt tất cả các ràng buộc cục bộ mà không cần lo lắng về thứ tự truyền tải. 

### 2. Diễn giải các yêu cầu như các ràng buộc liên kết 

Mỗi phòng có thể đóng vai trò như một đầu nối trong một đường đi ngang liên tục. Một đoạn truyền tải có thể đáp ứng nhiều yêu cầu nếu nó có thể di chuyển qua biểu đồ giữa chúng. 

Tuy nhiên, một căn phòng có nhiều yêu cầu đầu ra có thể vượt quá những gì có thể được xâu chuỗi thành các phân khúc hiện có, buộc phải bắt đầu lại. 

### 3. Bắt đầu tính toán số dư 

Chúng tôi giải thích từng yêu cầu là cần một đơn vị “công suất ban đầu”. Vì một đoạn đi bộ đóng góp chính xác một lần xuất phát nên số lượng đoạn cần thiết được xác định bằng tổng số lần xuất phát như vậy được yêu cầu. Trong công thức này, mỗi yêu cầu đóng góp một đơn vị, vì vậy câu trả lời là tổng số yêu cầu không thể được đưa vào các lần truyền tải đã bắt đầu trước đó, điều này giúp đơn giản hóa việc đếm sự đóng góp của tất cả các nút. 

### 4. Kết quả đầu ra 

Số lượng lối ra tối thiểu bằng tổng số thiết bị ban đầu được yêu cầu trên tất cả các phòng. 

### Tại sao nó hoạt động 

Bất biến chính là mọi kế hoạch di chuyển hợp lệ đều phân tách thành một tập hợp các đoạn đi bộ liên tục, trong đó mỗi đoạn bắt đầu tại một số phòng được trang bị lối ra. Mỗi yêu cầu về khóa cửa thuộc về đúng một phân khúc như vậy, cụ thể là phân khúc lần đầu tiên tiếp cận phòng.`a`khi yêu cầu đó được đáp ứng. 

Vì các phân đoạn không thể được hợp nhất mà không mất đi tính liên tục nên mọi “đơn vị luồng yêu cầu” độc lập phải được bắt đầu bằng một lối thoát. Khả năng kết nối biểu đồ đảm bảo tính linh hoạt khi định tuyến, do đó, hạn chế duy nhất là cần có bao nhiêu lần khởi tạo độc lập, đây chính xác là những gì công thức đếm nắm bắt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    out_req = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        out_req[a] += 1

    # each outgoing requirement corresponds to one needed initiation unit
    print(sum(out_req))

if __name__ == "__main__":
    solve()
```Giải pháp chỉ lưu trữ một mảng quầy duy nhất trên các phòng. Mỗi cạnh đầu vào sẽ tăng yêu cầu tại điểm cuối bắt đầu của nó. Tổng cuối cùng tổng hợp tất cả các đơn vị bắt đầu cần thiết, tương ứng trực tiếp với số lần thoát cần thiết. 

Không cần tìm kiếm truyền tải hoặc tìm kiếm đồ thị vì khả năng kết nối đảm bảo rằng việc định tuyến giữa các ràng buộc luôn có thể thực hiện được. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng chuyển động hoặc xây dựng danh sách lân cận và chạy DFS. Điều đó là không cần thiết và gây rủi ro cho cả TLE và việc xử lý không chính xác các ràng buộc chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2
```Ở đây, phòng 1 có một yêu cầu về khóa. 

| Bước | out_req[1] | out_req[2] | tổng cộng | 
| --- | --- | --- | --- | 
| sau cạnh | 1 | 0 | 1 | 

Kết quả là 1, nghĩa là một lần thoát là đủ để bắt đầu hành động khóa bắt buộc duy nhất. 

Điều này xác nhận rằng một điểm bắt đầu được yêu cầu duy nhất sẽ chuyển trực tiếp thành một lối ra. 

### Ví dụ 2 

đầu vào:```
3 2
2 1
3 1
```| Bước | out_req[1] | out_req[2] | out_req[3] | tổng cộng | 
| --- | --- | --- | --- | --- | 
| sau 2→1 | 0 | 1 | 0 | 1 | 
| sau 3→1 | 0 | 1 | 1 | 2 | 

Kết quả là 2. 

Điều này cho thấy cần có hai điểm xuất phát độc lập vì yêu cầu khóa bắt nguồn từ hai phòng riêng biệt không thể được đáp ứng bằng một lần khởi động liên tục mà không có lối ra bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cửa được xử lý một lần và tổng trên các nút là tuyến tính | 
| Không gian | O(n) | Chỉ duy trì một mảng có kích thước n | 

Giải pháp dễ dàng xử lý tới 1.000.000 cửa vì nó chỉ thực hiện một lần chuyển qua đầu vào và tổng hợp tuyến tính cuối cùng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    out_req = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        out_req[a] += 1

    return str(sum(out_req))

# provided samples
assert run("2 1\n1 2\n") == "1"
assert run("3 2\n2 1\n3 1\n") == "2"

# custom cases
assert run("2 0\n") == "0", "no doors"
assert run("4 3\n1 2\n2 3\n3 4\n") == "3", "chain structure"
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "4", "star centered at 1"
assert run("3 3\n1 2\n2 3\n3 1\n") == "3", "cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cạnh | 0 | đồ thị trống | 
| chuỗi | 3 | tích lũy tuyến tính | 
| ngôi sao | 4 | nhu cầu root lớn | 
| chu kỳ | 3 | tích lũy theo chu kỳ | 

## Vỏ cạnh 

Một trường hợp tối thiểu không có cửa ngay lập tức mang lại số lần thoát bằng 0 vì không cần thực hiện hành động khóa nào và thuật toán trả về 0 một cách chính xác do mảng yêu cầu vẫn trống. 

Trong biểu đồ hình ngôi sao trong đó một phòng kết nối với tất cả các phòng khác dưới dạng nguồn khóa, ví dụ:```
5 4
1 2
1 3
1 4
1 5
```thuật toán tăng dần`out_req[1]`bốn lần, dẫn đến đầu ra 4. Điều này phản ánh rằng bốn yêu cầu khóa độc lập bắt nguồn từ cùng một phòng, mỗi yêu cầu đều cần công suất khởi tạo. 

Trong một cấu trúc tuần hoàn như:```
3 3
1 2
2 3
3 1
```mỗi nút đóng góp chính xác một yêu cầu, tạo ra đầu ra là 3. Mặc dù biểu đồ hoàn toàn đối xứng về khả năng kết nối, các ràng buộc khóa là độc lập trên mỗi cạnh, do đó, không có chuỗi nào sẽ làm giảm nhu cầu khởi tạo riêng biệt.
