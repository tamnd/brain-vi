---
title: "CF 1070D - Xử lý rác thải"
description: "Mỗi ngày tạo ra một số lượng rác và mỗi đơn vị phải được vứt đi vào ngày nó xuất hiện hoặc vào ngày hôm sau."
date: "2026-06-15T13:49:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1070
codeforces_index: "D"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 1300
weight: 1070
solve_time_s: 331
verified: false
draft: false
---

[CF 1070D - Xử lý rác](https://codeforces.com/problemset/problem/1070/D) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngày tạo ra một số lượng rác và mỗi đơn vị phải được vứt đi vào ngày nó xuất hiện hoặc vào ngày hôm sau. Việc thải bỏ được thực hiện bằng cách đóng gói các thiết bị vào túi, trong đó mỗi túi có thể chứa được một dung tích cố định$k$, và túi sẽ được làm trống ngay vào ngày chuẩn bị. Mục đích là lên kế hoạch vận chuyển rác tối đa một ngày và nhóm lại thành các túi sao cho tổng số túi được sử dụng được giảm thiểu. 

Hạn chế chính là rác có thời gian tồn tại rất ngắn: nhiều nhất là hai ngày liên tiếp có sẵn. Điều này tạo ra sự tương tác cục bộ giữa các ngày liền kề chứ không phải là vấn đề lập kế hoạch chung. Đầu ra chỉ là số lượng tối thiểu các nhóm có giới hạn năng lực cần thiết để bao phủ tất cả các đơn vị trong các ràng buộc lân cận này. 

Kích thước đầu vào đạt$2 \cdot 10^5$, do đó, bất kỳ chiến lược bậc hai nào cố gắng mô phỏng các phép gán giữa tất cả các cặp ngày đều quá chậm. Các cách tiếp cận khả thi duy nhất là tuyến tính hoặc gần tuyến tính, vì khoảng$10^8$các thao tác đơn giản là giới hạn an toàn phía trên trong 3 giây về hiệu suất giống như Python C và chi phí Python đẩy chúng ta đến gần hơn$10^7$những hoạt động có ý nghĩa 

Một trường hợp khó tinh tế xuất hiện khi việc trì hoãn rác là cần thiết để tránh lãng phí dung lượng túi. Ví dụ: nếu ngày này có số lượng nhỏ và ngày tiếp theo có số lượng lớn, việc đóng gói một cách ngây thơ mỗi ngày riêng biệt có thể gây lãng phí công suất, trong khi việc chuyển các đơn vị tiến hoặc lùi trong khoảng thời gian cho phép có thể làm giảm số lượng túi. 

Một trường hợp khác xảy ra vào ngày cuối cùng. Rác thải phát sinh trong ngày$n$không thể bị trì hoãn thêm nữa, do đó, bất kỳ chiến lược nào ngầm giả định khả năng cân bằng trong tương lai đều phải xử lý ranh giới một cách cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các quyết định có thể xảy ra: mỗi ngày, quyết định số lượng đơn vị sẽ giữ lại cho ngày hôm sau và số lượng cần loại bỏ ngay lập tức, sau đó thử phân phối tất cả vào túi. Điều này đòi hỏi phải khám phá nhiều cấu hình theo cấp số nhân vì mỗi đơn vị có thể được chỉ định cho một trong hai ngày, sau đó việc nhóm mỗi ngày vào các túi sẽ giới thiệu một lớp tổ hợp khác. Ngay cả khi chúng ta hạn chế lập trình động hàng ngày trên các đơn vị còn sót lại, không gian trạng thái sẽ tăng lên với các giá trị mang có thể lên tới$k$, thực hiện chuyển tiếp$O(nk)$, quá lớn khi$k$tùy thuộc vào$10^9$. 

Việc đơn giản hóa cấu trúc xuất phát từ việc quan sát thấy rằng chúng ta không bao giờ cần phải mô phỏng rõ ràng các đơn vị riêng lẻ. Điều quan trọng là có bao nhiêu đơn vị trong ngày$i$được hoãn lại cho đến ngày$i+1$, vì đơn vị bị hoãn từ ngày$i$phải được xử lý cùng với ngày$i+1$. Sau khi chúng tôi ấn định số lượng đơn vị được chuyển tiếp, các quyết định còn lại trong ngày đó sẽ mang tính quyết định: chúng tôi chỉ cần tham lam lấp đầy các túi để giảm thiểu số lượng. 

Thông tin chi tiết cốt lõi về tham lam là trong mỗi ngày, chúng tôi muốn tối đa hóa việc tái sử dụng công suất bằng cách ghép các thiết bị còn sót lại từ ngày hôm trước với các thiết bị hiện tại. Thay vì theo dõi các nhiệm vụ riêng lẻ, chúng tôi theo dõi số lượng đơn vị vẫn đang "đang chờ xử lý" vào đầu mỗi ngày, sau đó tính toán số lượng đơn vị bổ sung phải được trì hoãn sang ngày tiếp theo để giảm thiểu số lượng túi cục bộ. 

Điều này làm giảm vấn đề thành một lần chuyển từ trái sang phải với một biến trạng thái đơn giản biểu thị rác chuyển tiếp chưa được xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(nk)$|$O(nk)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các ngày theo thứ tự trong khi vẫn duy trì số lượng đơn vị rác có sẵn để đóng gói vào ngày hiện tại, bao gồm cả rác mới và rác còn sót lại từ ngày hôm trước. 

1. Khởi tạo một biến`carry`về 0, đại diện cho rác đã bị hoãn lại từ ngày hôm trước và khởi tạo`ans`về 0 cho số lượng túi được sử dụng. 
2. Cho mỗi ngày$i$, thêm vào$a_i$ĐẾN`carry`, vì tất cả rác mới được tạo sẽ có sẵn cho hôm nay hoặc ngày mai. 
3. Tính xem hôm nay chúng ta có thể tạo ra bao nhiêu túi đầy bằng cách sử dụng tất cả rác có sẵn. Mỗi túi chứa tối đa$k$đơn vị, vậy số lượng túi chúng tôi có thể hoàn thành từ`carry`là`carry // k`. Thêm giá trị này vào câu trả lời và giảm`carry`tương ứng bằng cách sử dụng modulo$k$. 
4. Phần còn lại`carry`sau khi hình thành các túi đầy biểu thị các đơn vị còn sót lại không thể lấp đầy toàn bộ túi hôm nay. Những thức ăn thừa này phải được gộp chung với rác của ngày hôm sau hoặc được hoàn thành vào ngày hôm sau để được giữ nguyên. 
5. Trước khi chuyển sang ngày hôm sau, hãy đảm bảo rằng lượng thức ăn thừa không vượt quá mức có thể xử lý hợp lý. Vì mỗi đơn vị chỉ có thể chuyển tiếp tối đa một ngày nên mọi phần còn lại sẽ tự động có giá trị chuyển tiếp. 
6. Sau ngày xử lý$n$, tất cả còn lại`carry`phải được xử lý vào ngày cuối cùng. Thêm vào`carry // k`đến câu trả lời một lần nữa. 

Tính chính xác phụ thuộc vào thực tế là bất kỳ túi một phần nào vào một ngày nhất định phải được hoàn thành càng sớm càng tốt bằng cách sử dụng các đơn vị có sẵn, vì việc trì hoãn hoàn thành không làm tăng công suất nhưng có nguy cơ mất cơ hội ghép nối. 

### Tại sao nó hoạt động 

Tại bất kỳ ranh giới ngày nào, tất cả các đơn vị còn lại chỉ có thể tương tác với ngày hôm sau và không được tiếp tục. Do đó, chiến lược tối ưu tại địa phương là luôn lấp đầy càng nhiều túi đầy càng tốt ngay lập tức, bởi vì bất kỳ nhóm nào trì hoãn việc hình thành một túi đầy đều không thể cải thiện khả năng ghép đôi trong tương lai sau một ngày. Điều bất biến là sau ngày xử lý$i$, tất cả các đơn vị còn lại chính xác là những đơn vị phải được xem xét lại theo ngày$i+1$và tất cả các túi đã hoàn thành đều được cố định một cách tối ưu cho tiền tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    carry = 0
    ans = 0

    for x in a:
        carry += x
        ans += carry // k
        carry %= k

    ans += carry // k
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì một nhóm các đơn vị rác có sẵn đang hoạt động và tham lam trích xuất các túi đầy ở mỗi bước. Phép chia số nguyên nắm bắt chính xác số lượng túi hoàn chỉnh có thể được hình thành từ tình trạng sẵn có hiện tại và modulo chỉ giữ lại các đơn vị còn sót lại phải tương tác vào ngày hôm sau. Phần bổ sung cuối cùng xử lý việc dọn dẹp vào ngày cuối cùng. 

Một điểm tinh tế là chúng tôi không bao giờ quyết định rõ ràng có bao nhiêu đơn vị sẽ chuyển sang ngày hôm sau; điều này được mã hóa ngầm trong phần còn lại sau khi hình thành các nhóm đầy đủ, điều này là đủ vì chỉ phần còn lại mới có thể hưởng lợi từ sự kết hợp trong tương lai. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

đầu vào:```
3 2
3 2 1
```Chúng tôi theo dõi`carry`Và`ans`. 

| Ngày | một [tôi] | mang trước | túi được thêm vào | theo sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 1 | 1 | 2 | 
| 3 | 1 | 2 | 1 | 0 | 3 | 

Sau khi xử lý tất cả các ngày, câu trả lời là 3. 

Điều này cho thấy các đơn vị còn sót lại được truyền đi như thế nào, kết hợp bất cứ khi nào có thể nhưng luôn tôn trọng giới hạn một ngày. 

Bây giờ hãy xem xét một trường hợp mà sự trì hoãn là quan trọng. 

đầu vào:```
2 3
2 4
```| Ngày | một [tôi] | mang trước | túi được thêm vào | theo sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 2 | 0 | 
| 2 | 4 | 6 | 2 | 0 | 2 | 

Vào ngày đầu tiên, không có gì có thể tạo thành một túi đầy đủ, vì vậy mọi thứ đều được mang theo. Ngày thứ 2, cả 6 đơn vị kết hợp lại tạo ra 2 bao. Điều này chứng tỏ tại sao việc vận chuyển thức ăn thừa về phía trước là điều cần thiết để đóng gói tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ngày được xử lý một lần với các phép tính số học không đổi | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Quét tuyến tính dễ dàng phù hợp với giới hạn cho$n \le 2 \cdot 10^5$và mức sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    carry = 0
    ans = 0
    for x in a:
        carry += x
        ans += carry // k
        carry %= k
    ans += carry // k
    print(ans)

# provided sample
assert run("3 2\n3 2 1\n") == "3"

# minimum case
assert run("1 5\n4\n") == "0", "single day, no full bag"

# exact fit
assert run("1 5\n5\n") == "1", "one full bag"

# carry effect
assert run("2 3\n2 4\n") == "2", "optimal carry across days"

# large k
assert run("3 100\n50 60 70\n") == "1", "only final aggregation matters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5/4 | 0 | không có túi hình thành | 
| 1 5/5 | 1 | công suất chính xác | 
| 2 3 / 2 4 | 2 | cơ chế mang | 
| 3 100/50 60 70 | 1 | tổng hợp công suất lớn | 

## Vỏ cạnh 

Trong một ngày có ít hơn$k$các đơn vị, chẳng hạn như đầu vào`1 10 / 3`, bộ thuật toán`carry = 3`, tính toán không có túi đầy trong quá trình lặp và sau đó loại bỏ lần mang cuối cùng kể từ`3 // 10 = 0`, tạo ra 0 một cách chính xác. Tính đúng đắn xuất phát từ thực tế là không thể trì hoãn, vì vậy mọi đơn vị phải không được sử dụng trong điều kiện hình thành túi đầy đủ. 

Trong một ngày chính xác bằng$k$, chẳng hạn như`1 10 / 10`, thuật toán tạo thành chính xác một túi thông qua`carry // k = 1`và giảm mức mang về 0. Điều này xác nhận rằng việc trích xuất tham lam các túi đầy đủ ở mỗi bước xử lý chính xác các tình huống phù hợp chính xác mà không yêu cầu bất kỳ vỏ bọc đặc biệt nào.
