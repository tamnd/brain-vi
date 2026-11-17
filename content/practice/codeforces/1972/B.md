---
title: "CF 1972B - Trò chơi xu"
description: "Chúng ta được sắp xếp các đồng xu theo hình tròn. Mỗi đồng xu hướng lên hoặc úp xuống. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi phải chọn một đồng xu hiện đang ngửa, loại bỏ nó và sau đó lật hai đồng xu lân cận của nó."
date: "2026-06-09T02:05:57+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1972
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 942 (Div. 2)"
rating: 900
weight: 1972
solve_time_s: 236
verified: false
draft: false
---

[CF 1972B - Trò chơi tiền xu](https://codeforces.com/problemset/problem/1972/B) 

**Xếp hạng:** 900 
**Thẻ:** trò chơi 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các đồng xu theo hình tròn. Mỗi đồng xu hướng lên hoặc úp xuống. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi phải chọn một đồng xu hiện đang ngửa, loại bỏ nó và sau đó lật hai đồng xu lân cận của nó. Vòng tròn co lại khi đồng xu bị loại bỏ và cấu trúc liền kề thay đổi linh hoạt. 

Người chơi sẽ thua ngay lập tức nếu họ di chuyển và không có đồng xu hướng lên trên. Trò chơi luôn kết thúc vì mỗi nước đi sẽ làm giảm số xu. 

Nhiệm vụ là quyết định, đối với mỗi cấu hình ban đầu, liệu người chơi đầu tiên có thể giành chiến thắng hay không nếu cả hai bên chơi hoàn hảo. 

Các ràng buộc rất nhỏ, tối đa là 100 xu cho mỗi trường hợp thử nghiệm. Điều này cho phép chúng ta xem xét các chiến lược suy luận về các thuộc tính cấu trúc của trò chơi thay vì cần tính toán nặng nề hoặc tìm kiếm trên các trạng thái lớn. Về mặt lý thuyết, một mô phỏng cây trò chơi đầy đủ là có thể nhưng sẽ nhanh chóng trở nên không khả thi vì mỗi lần di chuyển sẽ phân nhánh trên tất cả các vị trí “lên” hợp lệ hiện tại và không gian trạng thái tăng theo cấp số nhân. 

Một trường hợp phức tạp phát sinh khi có rất ít đồng xu, đặc biệt là$n \le 3$. Trong những trường hợp như vậy, quy tắc lật bị suy biến vì việc loại bỏ một đồng xu có thể lật cùng một đồng xu hai lần hoặc loại bỏ tất cả các hiệu ứng lân cận. Một mô phỏng ngây thơ giả định cấu trúc ba lân cận ổn định sẽ tạo ra sự chuyển đổi không chính xác cho những trường hợp nhỏ này. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng toàn bộ trò chơi dưới dạng quy trình tối đa đệ quy trên tất cả các cấu hình có thể tiếp cận. Mỗi trạng thái được xác định bởi chuỗi nhị phân hình tròn hiện tại và trình phát sẽ di chuyển. Từ mỗi trạng thái, chúng tôi thử mọi cách loại bỏ đồng xu có thể và tính toán đệ quy kết quả của cấu hình kết quả. Cách tiếp cận này đúng vì nó mã hóa trực tiếp định nghĩa trò chơi, nhưng nó quá chậm vì số lượng trạng thái tăng theo cấp số nhân với$n$, và thậm chí đối với$n = 100$, số lượng cấu hình có thể tiếp cận là rất lớn. 

Quan sát quan trọng là trò chơi không nhạy cảm với vị trí chính xác của các đồng tiền tăng ngoài hiệu ứng ngang bằng về cấu trúc đơn giản. Mỗi nước đi sẽ loại bỏ một đồng xu lên và lật hai đồng xu lân cận, điều này chuyển đổi hiệu quả tính chẵn lẻ cục bộ và chuyển “hoạt động” xung quanh vòng tròn. Kiểu hoạt động này là đặc trưng của trò chơi khách quan, trong đó chỉ có tính chẵn lẻ của số lượng phần tử hoạt động là quan trọng chứ không phải sự sắp xếp chính xác của chúng. 

Sự đơn giản hóa mang tính quyết định là người chiến thắng chỉ phụ thuộc vào số lượng đồng xu lên là số lẻ hay số chẵn, với một sự điều chỉnh nhỏ cho trường hợp ban đầu không thể di chuyển được. Nếu không có đồng xu nào khi bắt đầu, người chơi đầu tiên sẽ thua ngay lập tức. Ngược lại, người chơi đầu tiên luôn có thể giành chiến thắng khi và chỉ khi số xu lên là số lẻ. 

Điều này hiệu quả vì mỗi nước đi sẽ loại bỏ chính xác một đồng xu lên và duy trì cấu trúc chẵn lẻ của tập hợp hoạt động còn lại sau khi lật. Các lần lật không làm thay đổi tính chẵn lẻ của tổng số đồng xu lên theo cách mà đối thủ có thể kiểm soát được, do đó trò chơi giảm xuống thành một trò chơi chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng trường hợp thử nghiệm và xử lý từng cấu hình một cách độc lập. Mỗi cấu hình đều độc lập vì không có sự tương tác giữa các trường hợp thử nghiệm. 
2. Đếm xem hiện tại có bao nhiêu đồng xu đang ngửa. Đây là biến trạng thái duy nhất quan trọng để xác định kết quả. 
3. Nếu không có đồng xu nào, người chơi đầu tiên không thể di chuyển và thua ngay lập tức. Đây là một vị trí buộc phải mất. 
4. Nếu không, hãy kiểm tra tính chẵn lẻ của số xu tăng. Nếu lẻ thì người chơi đầu tiên thắng; nếu chẵn thì người chơi thứ hai thắng. 
5. Xuất kết quả cho từng test. 

Lý do điều này có tác dụng là vì mỗi nước đi sẽ loại bỏ chính xác một đồng xu lên và thao tác lật không cho phép người chơi thay đổi tính chẵn lẻ của “tập hợp hoạt động hiệu quả” theo cách phá vỡ bất biến này. Trò chơi chuyển sang việc loại bỏ xen kẽ các đơn vị hoạt động hiệu quả và người chơi đầu tiên sẽ chiến thắng chính xác khi họ bắt đầu ở vị trí mà quá trình luân phiên này kết thúc có lợi cho họ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input().strip())
    s = input().strip()

    ups = s.count('U')

    if ups == 0:
        print("NO")
    elif ups % 2 == 1:
        print("YES")
    else:
        print("NO")
```Việc thực hiện trực tiếp tuân theo bất biến dẫn xuất. Chúng tôi chỉ tính số lượng xu tăng và áp dụng kiểm tra chẵn lẻ. Chuỗi được đọc một lần cho mỗi trường hợp thử nghiệm và việc đếm tuyến tính theo độ dài của nó. 

Một lỗi triển khai phổ biến là quên loại bỏ các ký tự dòng mới khi đọc chuỗi hoặc vô tình đọc sai nhiều dòng. Một vấn đề tế nhị khác là giả sử cấu trúc vòng tròn cần được mô hình hóa rõ ràng, nhưng điều đó không cần thiết một khi bất biến được xác định. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai cấu hình để minh họa cách áp dụng quy tắc chẵn lẻ. 

### Ví dụ 1 

đầu vào:```
5
UUDUD
```Chúng tôi tính toán số lượng xu tăng. 

| Bước | Tiểu bang | Số lần tăng | Quyết định | 
| --- | --- | --- | --- | 
| 1 | UUDUD | 3 | bắt đầu | 
| 2 | đếm ngược | 3 | tính toán | 
| 3 | chẵn lẻ | lẻ | Alice thắng | 

Vì số xu đổ lên là số lẻ nên người chơi đầu tiên sẽ thắng. 

Điều này cho thấy rằng việc thay đổi cấu trúc không quan trọng đối với việc phân loại kết quả. 

### Ví dụ 2 

đầu vào:```
5
UDDUD
```| Bước | Tiểu bang | Số lần tăng | Quyết định | 
| --- | --- | --- | --- | 
| 1 | UDDUD | 2 | bắt đầu | 
| 2 | đếm ngược | 2 | tính toán | 
| 3 | chẵn lẻ | thậm chí | Bob thắng | 

Mặc dù có nước đi tồn tại nhưng tính chẵn lẻ sẽ buộc người chơi đầu tiên phải thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm ký tự cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ lưu trữ chuỗi đầu vào và bộ đếm | 

Các ràng buộc cho phép tối đa 100 trường hợp kiểm thử với các chuỗi có độ dài lên tới 100. Quét tuyến tính trên mỗi trường hợp kiểm thử có thể dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        ups = s.count('U')
        if ups == 0:
            out.append("NO")
        elif ups % 2 == 1:
            out.append("YES")
        else:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run("""3
5
UUDUD
5
UDDUD
2
UU""") == """YES
NO
NO"""

# custom cases
assert run("""4
1
D
1
U
3
UUU
4
UUDD""") == """NO
YES
YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 Đ | KHÔNG | không thể di chuyển được | 
| 1 bạn | CÓ | nước đi thắng duy nhất | 
| ƯU | CÓ | nhiều trường hợp chẵn lẻ lẻ | 
| UUDD | KHÔNG | trường hợp chẵn lẻ hỗn hợp | 

## Vỏ cạnh 

Đối với một đồng xu, hành vi phụ thuộc hoàn toàn vào việc nó tăng hay giảm. Nếu nó bị hạ, người chơi đầu tiên không được di chuyển và thua ngay lập tức. Nếu nó hợp lệ, họ sẽ loại bỏ nó và trò chơi kết thúc ngay lập tức có lợi cho họ. Quy tắc chẵn lẻ khớp chính xác cả hai trường hợp. 

Đối với tất cả các đồng xu tăng lên, mỗi lần di chuyển sẽ loại bỏ một đồng xu đang hoạt động trong khi lật các đồng xu lân cận, nhưng tính chẵn lẻ vẫn là yếu tố quyết định. Nếu đếm là số lẻ thì người chơi đầu tiên luôn giữ được lợi thế nước đi cuối cùng. 

Đối với các cấu hình xen kẽ như UUDD, có các nước đi hợp lệ nhưng cấu trúc của các lần lật không làm thay đổi kết quả chẵn lẻ, do đó người chơi thứ hai sẽ thắng khi chơi tối ưu.
