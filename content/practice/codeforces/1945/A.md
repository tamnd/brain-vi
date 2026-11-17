---
title: "CF 1945A - Dựng trại"
description: "Chúng tôi được giao một nhóm người tham gia phải được phân vào các lều, trong đó mỗi lều có thể chứa tối đa ba người. Những người tham gia có ba loại với những ràng buộc khác nhau về cách họ sẵn sàng ở chung lều."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1945
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 935 (Div. 3)"
rating: 800
weight: 1945
solve_time_s: 55
verified: true
draft: false
---

[CF 1945A - Dựng trại](https://codeforces.com/problemset/problem/1945/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một nhóm người tham gia phải được phân vào các lều, trong đó mỗi lều có thể chứa tối đa ba người. Những người tham gia có ba loại với những ràng buộc khác nhau về cách họ sẵn sàng ở chung lều. 

Người hướng nội từ chối ở chung lều, vì vậy mỗi người hướng nội phải ở một mình trong lều. Những người hướng ngoại thì ngược lại: mỗi người nhất quyết muốn ở trong một lều đầy đủ chỉ có ba người, nghĩa là mỗi người hướng ngoại phải được nhóm cùng với hai người tham gia khác. Universals rất linh hoạt và có thể chiếm một chiếc lều một mình, với một người khác hoặc với hai người khác, miễn là kích thước lều không bao giờ vượt quá ba người. 

Nhiệm vụ là quyết định xem có thể phân công mọi người vào lều trong khi tôn trọng mọi ràng buộc hay không và nếu có thì giảm thiểu số lượng lều được sử dụng. 

Kích thước đầu vào lên tới 10^4 trường hợp thử nghiệm, với số lượng mỗi loại lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ sự mô phỏng nào đối với các cá nhân. Bất kỳ giải pháp đúng nào cũng phải giảm từng trường hợp thử nghiệm thành số học có thời gian không đổi. 

Một cách tiếp cận ngây thơ có thể cố gắng lấp đầy các lều một cách tham lam từng bước một, liên tục hình thành các nhóm hợp lệ cho đến khi hết người. Kiểu mô phỏng đó rất nguy hiểm vì thứ tự nhiệm vụ sẽ ảnh hưởng đến tính khả thi. Ví dụ, nếu những điều phổ quát được sử dụng quá sớm, sau này chúng ta có thể không thể hoàn thành các nhóm hướng ngoại đầy đủ. 

Một tình huống quan trọng xuất hiện khi người hướng ngoại không thể được xếp vào nhóm đầy đủ ba người ngay cả khi đã sử dụng tất cả các phổ quát: 

đầu vào:```
0 1 1
```Một người hướng ngoại và một người phổ thông không thể tạo thành một nhóm ba người hợp lệ, và người hướng ngoại không thể ở một mình hoặc ở trong một căn lều cục bộ. Đầu ra đúng là`-1`. Một thuật toán tham lam đặt những cái phổ quát ở nơi khác trước tiên có thể kết luận sai tính khả thi bằng cách khiến người hướng ngoại bị mắc kẹt. 

Một thất bại tinh tế khác nảy sinh khi người hướng nội tiêu tốn quá nhiều thời gian trong quá trình suy luận mà không tính đến thực tế là sau này có thể cần đến những cái chung để hoàn thiện các nhóm hướng ngoại. 

Những quan sát này gợi ý rằng chúng ta phải suy luận một cách toàn diện về việc có bao nhiêu điều phổ quát cần thiết đối với người hướng ngoại trước khi tính số lều còn lại. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là dựng lều theo đúng nghĩa đen. Chúng tôi liên tục cố gắng chia những người hướng ngoại thành các nhóm ba người, sau đó xếp những người hướng nội vào từng nhóm riêng lẻ, và cuối cùng phân bổ những kiến ​​thức chung cho những vị trí còn trống. Đối với mỗi trường hợp thử nghiệm, chúng ta có thể thử tất cả các cách phân phối phổ quát có thể có giữa các nhóm hướng ngoại và những chiếc lều còn sót lại. 

Tuy nhiên, không gian trạng thái tăng lên cùng với số lượng người tham gia. Ngay cả một mô phỏng đơn giản hóa xử lý từng người một lần vẫn có rủi ro O(a + b + c) cho mỗi trường hợp thử nghiệm, trở thành 10^14 thao tác trong trường hợp xấu nhất trên tất cả các đầu vào. Điều này là quá chậm. 

Cấu trúc của vấn đề gợi ý một sự phân rã trực tiếp hơn. Người hướng nội là cố định: mỗi người họ luôn tiêu thụ đúng một chiếc lều. Người hướng ngoại cũng cứng nhắc: họ phải thành lập các nhóm chính xác gồm ba người, vì vậy họ đóng góp b lều, nhưng có thể cần sự giúp đỡ từ các phổ quát nếu b không chia thành các bộ ba hoàn hảo. Những cái chung là nguồn tài nguyên linh hoạt duy nhất và vai trò của chúng là lấp đầy những chỗ còn thiếu trong các nhóm hướng ngoại và hình thành các nhóm cục bộ còn sót lại sau đó. 

Cái nhìn sâu sắc quan trọng là xử lý nhóm hướng ngoại trước tiên. Mỗi nhóm hướng ngoại hoàn toàn tiêu thụ một chiếc lều cỡ ba. Nếu b % 3 khác 0 thì 1 hoặc 2 người hướng ngoại còn lại yêu cầu những người phổ thông để hoàn thành nhóm của họ; mặt khác, cấu hình là không thể nếu không tồn tại đủ phổ quát. Sau khi làm hài lòng những người hướng ngoại, những chiếc lều phổ quát còn sót lại chỉ có thể giảm số lượng lều bằng cách xếp thành nhóm ba chiếc. 

Điều này làm giảm vấn đề về số học đơn giản hơn là tìm kiếm bài tập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a + b + c) mỗi bài kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đầu tiên hãy phân bổ một chiếc lều cho mỗi người hướng nội. Điều này là bắt buộc vì người hướng nội không thể chia sẻ. Chúng tôi bắt đầu câu trả lời với`a`. 
2. Tiếp theo, xử lý những người hướng ngoại theo nhóm ba người. Mỗi nhóm gồm ba người hướng ngoại sử dụng chính xác một chiếc lều, vì vậy chúng ta cộng`b // 3`để trả lời. 
3. Tính số dư`r = b % 3`. Điều này thể hiện những người hướng ngoại còn sót lại không thể tạo thành một nhóm hoàn chỉnh. 
4. Nếu`r > 0`, chúng ta phải sử dụng những cái phổ quát để hoàn thành nhóm hướng ngoại chưa hoàn chỉnh cuối cùng. Nếu chúng ta có ít hơn`3 - r`phổ quát sẵn có, chúng ta không thể thỏa mãn những ràng buộc của người hướng ngoại và quay trở lại`-1`. 
5. Nếu có thể hoàn thành nhóm cuối cùng, chúng tôi sẽ trừ đi những điểm phổ thông cần thiết. Nhóm chưa hoàn thiện vẫn đóng góp một lều nên chúng tôi thêm`1`để trả lời. 
6. Bây giờ chúng ta có những cái phổ quát còn lại. Chúng có thể được xếp tự do thành nhóm ba người, mỗi nhóm như vậy tạo thành một lều. Thêm vào`(c_remaining) // 3`để trả lời. 
7. Nếu đồ dùng phổ thông còn lại sau khi đóng gói thì vẫn chiếm lều, nhưng vì mỗi lều có thể chứa tối đa ba người nên những đồ dùng phổ thông còn sót lại luôn tạo thành tối đa một phần lều phụ. Tuy nhiên, điều này đã được tính bằng cách chia số nguyên vì 1-2 phổ còn lại vẫn cần một lều. 

Một công thức nhỏ gọn hơn xuất hiện: sau khi xử lý những người hướng ngoại, chỉ cần thêm những điểm chung còn lại vào các nhóm một cách tham lam bằng cách lấp đầy những chỗ còn sót lại, sau đó nhóm ba nhóm lại. 

### Tại sao nó hoạt động 

Thuật toán phân tách người tham gia bằng sự cứng nhắc. Người hướng nội và người hướng ngoại đặt ra những hạn chế về cấu trúc cố định, trong khi những cái chung là nguồn lực chỉ được sử dụng để sửa chữa những nhóm hướng ngoại chưa hoàn thiện hoặc lấp đầy năng lực còn sót lại. Điều bất biến là sau khi xử lý người hướng ngoại, mỗi chiếc lều đều được xác định đầy đủ hoặc có đúng một hoặc hai ô trống, và các vật dụng phổ quát luôn được đặt vào các ô này trước khi hình thành các ô mới. Vì những chiếc lều phổ quát không bao giờ có lợi nếu không được sử dụng trong một cấu trúc đã được lấp đầy một phần khi có thể tránh được một chiếc lều mới, nên việc đóng gói tham lam là tối ưu và không thể tăng số lượng lều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())

        # introverts always need their own tents
        ans = a

        # handle extroverts in full groups of 3
        ans += b // 3
        r = b % 3

        if r:
            need = 3 - r
            if c < need:
                print(-1)
                continue
            c -= need
            ans += 1

        # pack remaining universals
        ans += (c + 2) // 3

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ gán mỗi người hướng nội vào một lều riêng vì không có cấu hình nào khác hợp lệ. Sau đó, nó xếp những người hướng ngoại thành nhóm đầy đủ gồm ba người. Bất kỳ người hướng ngoại nào còn sót lại buộc phải tạo thêm một lều, nhưng chỉ khi có đủ phổ quát để hoàn thành nhóm đó. 

biểu hiện`(c + 2) // 3`là một cách tiêu chuẩn để đếm số lượng lều cần thiết để đóng gói các thiết bị phổ thông còn lại một cách tối ưu theo nhóm tối đa ba người. Nó ngầm xử lý các nhóm cuối cùng một phần mà không có trường hợp đặc biệt. 

Một sai lầm phổ biến là cố gắng đặt những cái phổ quát trước khi giải quyết những vấn đề còn sót lại của người hướng ngoại. Điều đó phá vỡ việc kiểm tra tính khả thi, bởi vì những cái phổ quát có thể được sử dụng ở những nơi không cần thiết, không còn cách nào để hoàn thành một nhóm hướng ngoại cần thiết. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

đầu vào:```
1
1 4 2
```| Bước | một | b | c | Hành động | lều | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 4 | 2 | người hướng nội | 1 | 
| máy lẻ đầy đủ | 1 | 4 | 2 | b//3 = 1 nhóm | 2 | 
| phần còn lại | 1 | 1 | 2 | r=1, cần 2 phổ quát | 2 | 
| điền | 1 | 1 | 0 | sử dụng 2 phổ quát | 3 | 
| cuối cùng | 1 | 1 | 0 | không còn phổ quát | 3 | 

Điều này cho thấy một nhóm hướng ngoại chưa hoàn thiện phải được hoàn thiện bằng cách sử dụng những cái chung, sau đó không còn sự linh hoạt nào nữa. 

### Ví dụ 2 

đầu vào:```
1
7 0 5
```| Bước | một | b | c | Hành động | lều | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 7 | 0 | 5 | người hướng nội | 7 | 
| máy lẻ | 7 | 0 | 5 | không hướng ngoại | 7 | 
| gói | 7 | 0 | 5 | phổ quát đóng gói | 9 | 

5 vũ trụ tạo thành một lều đầy đủ gồm 3 và một lều một phần gồm 2, tạo ra thêm hai lều. 

Những dấu vết này cho thấy thuật toán luôn ưu tiên các ràng buộc bắt buộc trước khi sử dụng các tài nguyên linh hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp dễ dàng xử lý 10^4 trường hợp thử nghiệm vì mỗi trường hợp được giải quyết trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        ans = a
        ans += b // 3
        r = b % 3
        if r:
            need = 3 - r
            if c < need:
                out.append("-1")
                continue
            c -= need
            ans += 1
        ans += (c + 2) // 3
        out.append(str(ans))
    return "\n".join(out)

# provided samples (partial inclusion for brevity)
assert run("10\n1 2 3\n1 4 1\n1 4 2\n1 1 1\n1 3 2\n19 7 18\n0 0 0\n7 0 0\n0 24 0\n1000000000 1000000000 1000000000") == \
"3\n-1\n3\n-1\n3\n28\n0\n7\n8\n1666666667"

# custom cases
assert run("1\n0 1 1") == "-1", "single extrovert impossible without enough universals"
assert run("1\n0 3 0") == "1", "exact full extrovert group"
assert run("1\n0 4 2") == "2", "mixed remainder packing"
assert run("1\n5 0 1") == "6", "introverts dominate"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 1 | -1 | hướng ngoại không thể hoàn thành | 
| 0 3 0 | 1 | phân nhóm chính xác những người hướng ngoại | 
| 0 4 2 | 2 | xử lý phần còn lại bằng phổ quát | 
| 5 0 1 | 6 | hướng nội thuần túy + ​​đóng gói phổ quát | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi người hướng ngoại để lại một hoặc hai phần còn lại sau khi nhóm thành ba nhóm. Ví dụ, hãy xem xét`0 1 2`. Người hướng ngoại độc thân không thể tạo thành một căn lều hợp lệ nếu không có thêm hai người nữa, vì vậy hai cái chung là hoàn toàn đủ. Thuật toán phát hiện`r = 1`, kiểm tra`c >= 2`, và hoàn thành một cái lều. 

Một trường hợp khác là khi những thứ phổ quát rất phong phú nhưng lại không phù hợp do nhu cầu khắt khe của người hướng nội. Vì`3 0 10`, người hướng nội sử dụng ba chiếc lều ngay lập tức và những điều phổ quát còn lại chỉ đơn giản là hình thành`10`mọi người vào`4`lều (3,3,3,1), cho tổng số`3 + 4 = 7`. 

Cuối cùng, khi không có người hướng ngoại nào cả, vấn đề giảm xuống là xếp người hướng nội và người phổ quát vào một cách độc lập. Thuật toán vẫn hoạt động vì nhánh hướng ngoại không đóng góp gì và những cái phổ quát được nhóm lại một cách tối ưu bằng cách sử dụng phép chia trần.
