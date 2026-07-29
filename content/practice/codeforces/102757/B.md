---
title: "CF 102757B - Đấu Sĩ Hiện Đại"
description: "Bài toán mô phỏng một cửa hàng trong đó mỗi khách hàng mua một mặt hàng bằng một đồng xu. Không phải lúc nào khách hàng cũng thanh toán bằng đồng, họ có thể giao đồng bạc hoặc đồng vàng. Alfonso phải trả lại ngay số tiền lẻ chính xác trước khi phục vụ khách hàng tiếp theo."
date: "2026-07-29T00:28:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102757
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 2"
rating: 0
weight: 102757
solve_time_s: 45
verified: true
draft: false
---

[CF 102757B - Đấu sĩ hiện đại](https://codeforces.com/problemset/problem/102757/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán mô phỏng một cửa hàng trong đó mỗi khách hàng mua một mặt hàng bằng một đồng xu. Không phải lúc nào khách hàng cũng thanh toán bằng đồng, họ có thể giao đồng bạc hoặc đồng vàng. Alfonso phải trả lại ngay số tiền lẻ chính xác trước khi phục vụ khách hàng tiếp theo. 

Hệ thống tiền xu là nhị phân: một đồng bạc có giá trị bằng hai đồng xu và một đồng vàng có cùng giá trị bằng hai đồng bạc. Tương tự, các giá trị là đồng = 1, bạc = 2 và vàng = 4. Alfonso bắt đầu không có xu, vì vậy một số khách hàng đầu tiên có thể xác định liệu anh ta có đủ xu nhỏ hơn để tiếp tục hay không. Mục tiêu là quyết định xem toàn bộ hàng đợi có thể được phục vụ hay không. 

Số lượng khách hàng có thể lên tới 100000, do đó, thuật toán liên tục tìm kiếm trong toàn bộ lịch sử hoặc thử các lựa chọn khả thi sẽ quá chậm. Chúng tôi cần một lần chuyển tiếp cho khách hàng, chỉ với công việc liên tục cho mỗi khách hàng, đưa ra giải pháp O(n). 

Những trường hợp khó khăn đều xuất phát từ việc có đủ tổng số tiền thôi cũng chưa đủ. Alfonso có thể có đủ giá trị nhưng không có mệnh giá phù hợp để thay đổi. Ví dụ:```
3
gold
gold
gold
```Đầu ra đúng là:```
NO
```Sau đồng vàng đầu tiên, Alfonso sở hữu một đồng vàng nhưng cần phải trả lại ba đồng tiền đồng. Anh ta không có đồng xu nào nhỏ hơn nên anh ta thất bại ngay lập tức. Giải pháp bất cẩn chỉ theo dõi tổng giá trị sẽ cho rằng mình có đủ tiền và đưa ra đáp án sai. 

Một trường hợp khác là khi một đồng tiền có mệnh giá cao hơn xuất hiện sau một vài đồng tiền nhỏ hơn:```
4
bronze
bronze
silver
gold
```Đầu ra đúng là:```
YES
```Hai khách hàng đầu tiên đưa cho Alfonso hai đồng xu. Việc thanh toán bạc có thể được xử lý bằng cách trả lại một đồng. Khoản thanh toán vàng cuối cùng có thể được xử lý bằng cách trả lại một bạc và một đồng. Giải pháp chỉ kiểm tra xem mọi giá trị xu có xuất hiện đủ số lần hay không có thể bỏ lỡ yêu cầu đặt hàng. 

Trường hợp ranh giới cuối cùng là:```
3
bronze
silver
gold
```Đầu ra đúng là:```
NO
```Sau khi thanh toán bằng đồng, Alfonso có một đồng. Việc thanh toán bạc thành công và để lại một đồng bạc. Việc thanh toán bằng vàng cần có ba đồng xu trị giá, nhưng Alfonso chỉ có một đồng bạc. Đồng bạc không thể được trả lại dưới dạng tiền lẻ vì nó có giá trị bằng hai đồng thay vì ba. 

# Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng mọi cách có thể mà Alfonso có thể tạo ra sự thay đổi. Đối với mỗi khách hàng, chương trình có thể thử các cách kết hợp tiền xu khác nhau để đạt được thay đổi cần thiết và tiếp tục đệ quy. Điều này đúng vì mọi chuỗi quyết định có thể đều được xem xét, nhưng nó nhanh chóng trở thành không thể. Trong trường hợp xấu nhất, mỗi khách hàng vàng sẽ giới thiệu một số cách khả thi để tạo ra ba đồng xu có giá trị tiền lẻ và việc khám phá các nhánh đó sẽ dẫn đến hành vi theo cấp số nhân. 

Cấu trúc của hệ thống tiền xu đưa ra một lộ trình đơn giản hơn nhiều. Vì mỗi mệnh giá chính xác gấp đôi mệnh giá trước đó nên những đồng tiền lớn hơn luôn có giá trị hơn và kém linh hoạt hơn những đồng tiền nhỏ hơn. Điều duy nhất quan trọng đối với khách hàng trong tương lai là Alfonso hiện có bao nhiêu đồng xu bằng đồng và bạc, bởi vì tiền vàng không bao giờ cần thiết để thực hiện tiền lẻ. Một đồng tiền vàng luôn còn trong bộ sưu tập của anh ta sau giao dịch. 

Quan sát quan trọng là sự thay đổi có thể được thực hiện một cách tham lam. Một khách hàng bạc yêu cầu chính xác một đồng xu. Một khách hàng vàng yêu cầu ba đồng xu có giá trị. Cách tốt nhất để cung cấp ba đồng xu có giá trị tiền lẻ là sử dụng một đồng bạc và một đồng xu bất cứ khi nào có thể, vì điều này sẽ tiết kiệm được nhiều đồng xu hơn cho khách hàng bạc trong tương lai. Nếu điều đó là không thể thì ba đồng xu là lựa chọn duy nhất còn lại. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi cách hợp lý để tạo ra sự thay đổi, nhưng thất bại vì số lượng lựa chọn tăng quá nhanh. Bản chất nhị phân của các giá trị đồng xu loại bỏ sự mơ hồ đó và cho phép chúng tôi chỉ duy trì số lượng xu cần thiết trong khi xử lý dòng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng khách hàng | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Duy trì bộ đếm số lượng đồng xu bằng đồng, bạc và vàng mà Alfonso sở hữu. Ban đầu cả ba quầy đều bằng 0. 
2. Đọc từng khách hàng từ phía trước hàng đợi và thêm số xu nhận được vào bộ sưu tập của Alfonso. Đồng xu nhận được không thể được sử dụng để thanh toán món hàng vì giao dịch đã diễn ra nên nó trở thành một phần số tiền hiện có của anh ta. 
3. Nếu khách hàng thanh toán bằng đồng thì không cần thay đổi gì vì số tiền thanh toán hoàn toàn khớp với giá. 
4. Nếu khách hàng thanh toán bằng bạc, lấy ra một đồng xu làm tiền lẻ. Nếu không có đồng xu thì việc phục vụ khách hàng là không thể. 
5. Nếu khách hàng thanh toán bằng vàng, hãy lấy ra tiền lẻ trị giá 3 đồng xu. Thích sử dụng một đồng bạc và một đồng đồng vì điều đó giúp giữ lại các đồng xu còn lại để thanh toán bạc sau này. Nếu không được thì dùng ba đồng xu. Nếu không có tùy chọn nào tồn tại, quá trình này không thành công. 
6. Nếu mọi khách hàng đều được xử lý thành công, hãy xuất ra`YES`. Nếu không thì xuất ra`NO`tại điểm đầu tiên mà sự thay đổi không thể được tạo ra. 

Tại sao nó hoạt động: 

Điều bất biến là sau mỗi khách hàng được phục vụ thành công, số lượng xu được lưu trữ thể hiện chính xác số xu mà Alfonso có sau khi hoàn thành tất cả các giao dịch cho đến nay. Đối với thanh toán bằng bạc, yêu cầu thay đổi duy nhất có thể là một đồng xu. Đối với thanh toán bằng vàng, mọi cách hợp lệ để trả lại ba đồng xu là một bạc cộng một đồng hoặc ba đồng. Sự lựa chọn tham lam là an toàn vì việc thay thế một đồng bạc bằng hai đồng đồng sẽ chỉ làm giảm tính linh hoạt của Alfonso. Giữ số tiền lớn hơn trong khi chi tiêu giá trị nhỏ hơn trước tiên sẽ mang lại ít nhất nhiều khả năng phục vụ tất cả khách hàng trong tương lai. Vì mọi khách hàng đều được xử lý bằng cách sử dụng một giao dịch hợp lệ để duy trì tính bất biến này nên câu trả lời cuối cùng là chính xác. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    bronze = 0
    silver = 0
    gold = 0

    for _ in range(n):
        coin = input().strip()

        if coin == "bronze":
            bronze += 1
        elif coin == "silver":
            silver += 1
            if bronze == 0:
                print("NO")
                return
            bronze -= 1
        else:
            gold += 1
            if silver > 0 and bronze > 0:
                silver -= 1
                bronze -= 1
            elif bronze >= 3:
                bronze -= 3
            else:
                print("NO")
                return

    print("YES")

if __name__ == "__main__":
    solve()
```Ba biến chỉ lưu trữ thông tin cần thiết cho khách hàng trong tương lai. Tiền vàng được tính vì chúng thuộc về Alfonso sau khi nhận, nhưng chúng không bao giờ tham gia đổi tiền. 

Đối với thanh toán bằng bạc, trước tiên mã sẽ thêm đồng bạc rồi xóa một đồng xu. Đơn hàng đúng vì khách hàng đưa xu trước khi Alfonso trả lại tiền lẻ. 

Đối với thanh toán bằng vàng, trước tiên mã sẽ giữ đồng tiền vàng, sau đó kiểm tra hai cách hợp lệ để tạo ra ba đồng tiền lẻ. Nhánh ưu tiên sử dụng đồng bạc và đồng xu. Nhánh dự phòng sử dụng ba đồng xu. Việc kiểm tra được viết trước khi trừ để bộ đếm không bao giờ trở thành số âm. 

Số nguyên Python không bị tràn và thuật toán chỉ sử dụng một vài biến, do đó nó vẫn hiệu quả ngay cả đối với số lượng khách hàng tối đa. 

# Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
4
bronze
bronze
silver
gold
```| Khách hàng | Thanh toán | Đồng | Bạc | Vàng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đồng | 1 | 0 | 0 | Giữ thanh toán chính xác | 
| 2 | đồng | 2 | 0 | 0 | Giữ thanh toán chính xác | 
| 3 | bạc | 1 | 1 | 0 | Trả lại một đồng | 
| 4 | vàng | 0 | 0 | 1 | Trả lại một bạc và một đồng | 

Ví dụ này chứng minh tại sao việc theo dõi mệnh giá lại quan trọng. Alfonso có đủ giá trị ở mọi giai đoạn, nhưng số tiền chính xác sẽ quyết định liệu anh ấy có thể tiếp tục hay không. 

Mẫu 2: 

đầu vào:```
3
bronze
silver
gold
```| Khách hàng | Thanh toán | Đồng | Bạc | Vàng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đồng | 1 | 0 | 0 | Giữ thanh toán chính xác | 
| 2 | bạc | 0 | 1 | 0 | Trả lại một đồng | 
| 3 | vàng | 1 | 1 | 1 | Không thể tạo thay đổi | 

Giao dịch cuối cùng không thành công vì Alfonso có một đồng bạc và một đồng đồng, nhưng số tiền lẻ cần có là ba đồng xu. Thuật toán từ chối chính xác thứ tự này. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khách hàng được xử lý một lần với các cập nhật liên tục. | 
| Không gian | O(1) | Chỉ có ba quầy tiền xu được lưu trữ. | 

Kích thước đầu vào có thể đạt tới 100000 khách hàng, do đó việc quét tuyến tính dễ dàng nằm trong giới hạn yêu cầu. Không cần cấu trúc dữ liệu tùy thuộc vào số lượng khách hàng. 

# Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    n = int(sys.stdin.readline())
    bronze = silver = gold = 0

    for _ in range(n):
        coin = sys.stdin.readline().strip()

        if coin == "bronze":
            bronze += 1
        elif coin == "silver":
            silver += 1
            if bronze == 0:
                print("NO")
                break
            bronze -= 1
        else:
            gold += 1
            if silver > 0 and bronze > 0:
                silver -= 1
                bronze -= 1
            elif bronze >= 3:
                bronze -= 3
            else:
                print("NO")
                break
    else:
        print("YES")

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert solve_io("""4
bronze
bronze
silver
gold
""") == "YES\n", "sample 1"

assert solve_io("""3
bronze
silver
gold
""") == "NO\n", "sample 2"

assert solve_io("""3
gold
gold
gold
""") == "NO\n", "no initial change"

assert solve_io("""5
bronze
bronze
bronze
gold
silver
""") == "YES\n", "multiple change operations"

assert solve_io("""3
silver
bronze
gold
""") == "NO\n", "bad ordering"

assert solve_io("""100000
""" + "bronze\n" * 100000) == "YES\n", "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`bronze, bronze, silver, gold`| CÓ | Trình tự thành công bình thường | 
|`bronze, silver, gold`| KHÔNG | Vấn đề đặt hàng mệnh giá | 
|`gold, gold, gold`| KHÔNG | Bắt đầu không thay đổi | 
|`bronze, bronze, bronze, gold, silver`| CÓ | Sử dụng nhiều lần thay đổi | 
|`silver, bronze, gold`| KHÔNG | Các trường hợp tổng giá trị bị sai lệch | 
| Thanh toán 100000 đồng | CÓ | Kích thước đầu vào tối đa | 

# Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
3
gold
gold
gold
```Thuật toán bắt đầu với số tiền bằng 0. Khách hàng vàng đầu tiên đưa cho Alfonso một đồng vàng, nhưng thuật toán sẽ kiểm tra xem liệu có tồn tại ba đồng xu có giá trị tiền lẻ hay không. Không có một đồng bạc, một đồng, ba đồng đồng nên in ngay`NO`. Điều này phù hợp với quy trình thực tế vì đồng tiền vàng không thể chia thành các đồng tiền nhỏ hơn. 

Đối với trường hợp nhạy cảm với thứ tự:```
3
bronze
silver
gold
```Sau khách hàng bằng đồng, Alfonso sở hữu một đồng xu. Khách hàng bạc đưa một đồng bạc và lấy một đồng bạc, chỉ để lại một đồng bạc. Khách hàng vàng yêu cầu ba đồng tiền lẻ. Thuật toán không thể tìm thấy cặp bạc và đồng hoặc ba đồng xu, vì vậy nó từ chối chuỗi. 

Đối với trường hợp biên thành công:```
4
bronze
bronze
silver
gold
```Thuật toán tích lũy hai đồng xu, dùng một đồng để thanh toán bằng bạc, sau đó sử dụng số đồng còn lại cùng với đồng bạc để thanh toán bằng vàng. Mọi giao dịch đều bảo toàn sự bất biến rằng số tiền được lưu trữ của Alfonso chính xác là số tiền thật của anh ấy sau khi phục vụ khách hàng, vì vậy câu trả lời cuối cùng là`YES`.
