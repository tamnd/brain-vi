---
title: "CF 902A - Thăm bạn bè"
description: "Chúng ta được cung cấp một bộ dịch chuyển tức thời được đặt dọc theo trục số một chiều. Lợn xuất phát ở vị trí 0 và muốn tới vị trí m."
date: "2026-06-15T11:52:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 902
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 453 (Div. 2)"
rating: 1100
weight: 902
solve_time_s: 316
verified: true
draft: false
---

[CF 902A - Thăm bạn bè](https://codeforces.com/problemset/problem/902/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ dịch chuyển tức thời được đặt dọc theo trục số một chiều. Lợn xuất phát ở vị trí 0 và muốn tới vị trí m. Mỗi người dịch chuyển được đặt tại một tọa độ ai nào đó và nếu Pig đến tọa độ đó, anh ta có thể chọn bất kỳ điểm đến nào trong khoảng từ ai đến bi. 

Các dịch chuyển tức thời được trình bày theo thứ tự trong đó vị trí bắt đầu của chúng không bao giờ giảm khi chúng ta di chuyển xuống danh sách. Thứ tự này quan trọng vì nó cho phép chúng ta xử lý chúng từ trái sang phải mà không cần xem xét lại cấu trúc trước đó. 

Nhiệm vụ là xác định xem có tồn tại một chuỗi sử dụng dịch chuyển tức thời bắt đầu từ 0 và cuối cùng đạt đến m hay không. 

Những hạn chế là nhỏ. Với tối đa 100 lần dịch chuyển tức thời, ngay cả mô phỏng O(n²) cũng có thể dễ dàng vượt qua, nhưng cấu trúc cho thấy quét tuyến tính đơn giản hơn là đủ. Khó khăn chính không phải là thời gian tính toán mà là hiểu được khả năng tiếp cận phát triển như thế nào khi có các dịch vụ dịch chuyển mới. 

Một sự hiểu lầm ngây thơ thường xuất phát từ việc xử lý từng dịch chuyển một cách độc lập. Ví dụ: giả sử rằng có thể dịch chuyển tức thời tại ai ngay lập tức có nghĩa là bạn phải sử dụng nó hoặc giả sử bạn chỉ có thể sử dụng dịch chuyển tức thời theo thứ tự sử dụng nghiêm ngặt chứ không phải tính khả dụng. Một sai lầm tinh vi khác là bỏ qua rằng dịch chuyển tức thời có thể mở rộng phạm vi ra ngoài vị trí của chính nó, tạo điều kiện cho các dịch chuyển tức thời sau này có thể sử dụng được. 

Một trường hợp thất bại cụ thể trông như thế này: 

đầu vào:```
2 5
0 1
2 5
```Một cách tiếp cận bất cẩn có thể cố gắng xâu chuỗi một cách nghiêm ngặt và không nhận ra rằng chỉ đạt tối đa 1 sẽ không cho phép truy cập vào dịch chuyển tức thời thứ hai ở mức 2, vì vậy câu trả lời là KHÔNG. Tuy nhiên, cách tiếp cận theo dõi phạm vi tiếp cận chính xác sẽ thấy rằng giới hạn dịch chuyển tức thời đầu tiên đạt đến 1, điều này là không đủ nên quá trình sẽ dừng lại. 

Một trường hợp cạnh khác: 

đầu vào:```
2 5
0 3
1 2
```Mặc dù dịch chuyển tức thời thứ hai có vẻ “lạc hậu” về mặt hữu ích, nhưng sẽ không liên quan nếu chúng ta đã tiến xa hơn với dịch chuyển đầu tiên. Logic đúng phải luôn ưu tiên phần mở rộng có thể tiếp cận tối đa chứ không phải dịch chuyển tức thời gặp phải gần đây nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi đây là một vấn đề về khả năng tiếp cận trong đó từ vị trí hiện tại, chúng tôi thử mọi dịch chuyển tức thời có thể sử dụng được, khám phá đệ quy tất cả các điểm đến có thể có trong khoảng thời gian cho phép. Điều này xây dựng một biểu đồ một cách hiệu quả trong đó mỗi điểm trong mỗi khoảng là một nút và dịch chuyển tức thời tạo ra các kết nối có hướng dày đặc. Mặc dù đúng về mặt khái niệm, nhưng điều này bùng nổ vì mỗi dịch chuyển tức thời có thể tạo ra một loạt các điểm có thể tiếp cận liên tục và ngay cả khi bị rời rạc, chúng ta có thể quay lại nhiều lần các trạng thái. Trong trường hợp xấu nhất, việc khám phá tất cả các cách kết hợp sử dụng dịch chuyển tức thời sẽ dẫn đến hành vi theo cấp số nhân. 

Quan sát quan trọng là chúng ta không bao giờ cần theo dõi tất cả các vị trí có thể tiếp cận, chỉ điểm xa nhất mà Pig hiện có thể tiếp cận. Nếu chúng ta có thể tiếp cận ai, thì cách sử dụng dịch chuyển tức thời tốt nhất có thể là mở rộng phạm vi tiếp cận tới bi. Vì mọi chuyển động đều đơn điệu về mặt vị trí (chúng ta không bao giờ cần phải lùi lại), nên vấn đề giảm xuống mức tham lam mở rộng biên giới. 

Chúng tôi xử lý dịch chuyển tức thời theo thứ tự tăng dần của ai. Bất cứ khi nào điểm bắt đầu của dịch chuyển tức thời nằm trong phạm vi có thể tiếp cận hiện tại của chúng tôi, chúng tôi có thể sử dụng điểm đó để mở rộng phạm vi đó. Nếu dịch chuyển tức thời bắt đầu vượt quá những gì chúng tôi có thể tiếp cận hiện tại, chúng tôi chưa thể sử dụng nó và vì dịch chuyển tức thời trong tương lai thậm chí còn bắt đầu xa hơn (do đặt hàng), chúng tôi sẽ ngừng kiểm tra các tiện ích mở rộng tiếp theo khi đạt đến khoảng cách như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái) | Hàm mũ | Cao | Quá chậm | 
| Tham lam mở rộng phạm vi tiếp cận | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một giá trị duy nhất đại diện cho vị trí xa nhất mà Pig hiện có thể đạt tới. 

1. Khởi tạo vị trí có thể truy cập là 0 vì Pig bắt đầu ở đó. 
2. Lặp lại từng lần dịch chuyển theo thứ tự nhất định. 
3. Nếu điểm bắt đầu của dịch chuyển tức thời ai lớn hơn vị trí có thể tiếp cận hiện tại, hãy ngừng xử lý các dịch chuyển tiếp theo. Điều này có tác dụng vì những lần dịch chuyển sau này thậm chí còn bắt đầu xa hơn về bên phải và do đó cũng không thể truy cập được. 
4. Nếu ai nằm trong phạm vi có thể truy cập, hãy cập nhật vị trí có thể truy cập thành max(phạm vi tiếp cận hiện tại, bi). Điều này phản ánh việc sử dụng dịch chuyển tức thời một cách tối ưu để mở rộng sang phải càng xa càng tốt. 
5. Sau khi xử lý tất cả các dịch chuyển tức thời có thể sử dụng được, hãy kiểm tra xem vị trí có thể tiếp cận ít nhất là m hay không. Nếu có, Pig có thể liên lạc được với người bạn đó; nếu không thì anh ta không thể. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng vị trí có thể tiếp cận là điểm ngoài cùng bên phải tối đa mà Pig có thể tiếp cận bằng cách sử dụng bất kỳ chuỗi dịch chuyển tức thời nào từ tiền tố đã xử lý. Bởi vì dịch chuyển tức thời được sắp xếp theo ai, một khi chúng ta gặp một dịch chuyển tức thời có điểm xuất phát nằm ngoài tầm với hiện tại, thì dịch chuyển tức thời trong tương lai sẽ không thể được kích hoạt. Trong số tất cả các dịch chuyển tức thời có thể sử dụng được, lấy bi một cách tham lam là tối ưu vì bất kỳ lựa chọn trung gian nào trong [ai, bi] chỉ có thể làm giảm hoặc bằng phạm vi tiếp cận trong tương lai chứ không bao giờ cải thiện nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    reach = 0
    
    for _ in range(n):
        a, b = map(int, input().split())
        
        if a > reach:
            break
        
        if a <= reach:
            if b > reach:
                reach = b
    
    print("YES" if reach >= m else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp ý tưởng tham lam. Biến`reach`lưu trữ vị trí xa nhất có thể tiếp cận cho đến nay. Việc nghỉ ngơi sớm rất quan trọng: một khi dịch chuyển tức thời nằm ngoài tầm với, những dịch chuyển sau đó không thể giúp đỡ do ai đã được sắp xếp. Bản cập nhật tối đa đảm bảo chúng tôi luôn mở rộng phạm vi tiếp cận nhiều nhất có thể. 

Một lỗi phổ biến là loại bỏ phần ngắt và tiếp tục quét, điều này vẫn đúng nhưng chậm hơn ở các biến thể lớn hơn. Một sai lầm khác là cập nhật phạm vi tiếp cận bằng ai thay vì bi, điều này sẽ bỏ qua khả năng mở rộng của mỗi dịch chuyển tức thời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5
0 2
2 4
3 5
```| Bước | ai | bi | Tiếp cận trước | Quyết định | Tiếp cận sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 0 | có thể sử dụng được | 2 | 
| 2 | 2 | 4 | 2 | có thể sử dụng được | 4 | 
| 3 | 3 | 5 | 4 | có thể sử dụng được | 5 | 

Pig dần dần mở rộng khoảng cách có thể tiếp cận cho đến khi đạt 5. 

Điều này xác nhận tính bất biến mà tầm với luôn phản ánh vị trí tốt nhất có thể đạt được sau mỗi lần dịch chuyển có thể sử dụng được. 

### Mẫu 2 

đầu vào:```
2 5
0 3
4 5
```| Bước | ai | bi | Tiếp cận trước | Quyết định | Tiếp cận sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | có thể sử dụng được | 3 | 
| 2 | 4 | 5 | 3 | bị chặn | 3 | 

Dịch chuyển thứ hai không bao giờ sử dụng được vì Pig không thể đến điểm xuất phát. Điều này chứng tỏ tại sao việc kiểm soát dựa trên phạm vi tiếp cận là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần dịch chuyển được xử lý một lần cho đến khi đạt được khối quét tiếp theo | 
| Không gian | O(1) | Chỉ có một biến duy nhất được sử dụng để theo dõi phạm vi tiếp cận | 

Các ràng buộc cho phép dịch chuyển tối đa 100 lần, do đó, ngay cả việc quét tuyến tính cũng thấp hơn nhiều so với giới hạn. Giải pháp có hiệu quả là liên tục trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import *
    
    n, m = map(int, sys.stdin.readline().split())
    reach = 0
    
    for _ in range(n):
        a, b = map(int, sys.stdin.readline().split())
        if a > reach:
            break
        if b > reach:
            reach = b
    
    return "YES\n" if reach >= m else "NO\n"

# provided sample 1
assert run("3 5\n0 2\n2 4\n3 5\n") == "YES\n"

# sample 2
assert run("2 5\n0 3\n4 5\n") == "NO\n"

# custom case: already at target
assert run("1 1\n0 5\n") == "YES\n"

# custom case: cannot start
assert run("2 10\n1 5\n2 9\n") == "NO\n"

# custom case: chained reach
assert run("4 10\n0 2\n2 5\n5 7\n7 10\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bước nhảy lớn duy nhất | CÓ | trường hợp thành công ngay lập tức | 
| bắt đầu không thể truy cập | KHÔNG | dịch chuyển đầu tiên không sử dụng được | 
| bảo hiểm xích | CÓ | khai triển tham lam đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi lần dịch chuyển đầu tiên không bắt đầu từ 0. Ví dụ: nếu ai đầu tiên là 1 thì Pig hoàn toàn không thể di chuyển nên câu trả lời ngay lập tức là KHÔNG. Thuật toán xử lý việc này một cách tự nhiên vì phạm vi tiếp cận bắt đầu từ 0 và lần kiểm tra đầu tiên a > phạm vi tiếp cận sẽ gây ra sự chấm dứt. 

Một trường hợp khác là khi nhiều dịch chuyển tức thời chồng chéo lên nhau. Ngay cả khi dịch chuyển sau đó có khoảng thời gian ngắn hơn, điều đó không thành vấn đề vì phạm vi tiếp cận chỉ tăng lên thông qua các bản cập nhật tối đa. Thuật toán bỏ qua các hồi quy vô ích một cách an toàn vì nó không bao giờ làm giảm phạm vi tiếp cận.
