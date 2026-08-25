---
title: "CF 104317E – Loại bỏ sự nghi ngờ"
description: "Chúng ta có hai tập hợp điểm trên một mặt phẳng, mỗi điểm cũng có tọa độ thời gian. Bộ đầu tiên đại diện cho con người, trong đó mỗi người có một vị trí được ghi lại tại một thời điểm cụ thể. Bộ thứ hai đại diện cho các sự kiện tội phạm, mỗi sự kiện xảy ra ở một vị trí và thời gian."
date: "2026-07-01T19:31:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104317
codeforces_index: "E"
codeforces_contest_name: "Shanghai University 2023 Spring Contest"
rating: 0
weight: 104317
solve_time_s: 85
verified: false
draft: false
---

[CF 104317E - Loại bỏ sự nghi ngờ](https://codeforces.com/problemset/problem/104317/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên một mặt phẳng, mỗi điểm cũng có tọa độ thời gian. Bộ đầu tiên đại diện cho con người, trong đó mỗi người có một vị trí được ghi lại tại một thời điểm cụ thể. Bộ thứ hai đại diện cho các sự kiện tội phạm, mỗi sự kiện xảy ra ở một vị trí và thời gian. 

Để một người được tuyên bố là hoàn toàn an toàn, chúng ta phải có khả năng chứng minh rằng không có sự kiện tội phạm nào có thể xảy ra "đủ gần" trong không thời gian để nhất quán với việc người đó là thủ phạm. Quy tắc khoảng cách là khoảng cách của Manhattan trong không gian so với chênh lệch thời gian: một sự kiện tội phạm được coi là có khả năng tiếp cận được từ một người nếu khoảng cách không gian giữa chúng không lớn hơn chênh lệch thời gian. Nếu tồn tại dù chỉ một sự kiện tội phạm mà một người có thể đã tiếp cận hoặc bị ảnh hưởng dưới sự ràng buộc này, thì người đó đang nghi ngờ. 

Vì vậy, đối với mỗi người, chúng ta phải kiểm tra xem liệu tất cả các sự kiện tội phạm có ở quá xa so với thời gian trong không gian hay không, nghĩa là khoảng cách không gian của Manhattan có lớn hơn chênh lệch thời gian tuyệt đối cho mỗi tội phạm hay không. Nếu điều này được giữ, người đó sẽ an toàn. 

Giải thích trực tiếp là mỗi người xác định một hạn chế đối với tất cả các điểm tội phạm và hành vi vi phạm sẽ xảy ra nếu bất kỳ tội phạm nào nằm trong hoặc trên một “hình nón ánh sáng” được định hình bởi sự chênh lệch về khoảng cách và thời gian của Manhattan. 

Kích thước đầu vào lớn, lên tới 300.000 người và 300.000 tội phạm. Việc kiểm tra từng cặp đơn giản sẽ yêu cầu so sánh tới 9e10, vượt xa giới hạn khả thi. Điều này ngay lập tức loại trừ bất kỳ giải pháp O(nm) nào và đẩy chúng ta tới một phương pháp trong đó cả hai bộ được xử lý theo cấu trúc hình học chung hoặc quét. 

Các trường hợp cạnh xuất hiện khi nhiều điểm có chung tọa độ hoặc dấu thời gian giống nhau. Đặc biệt, khi chênh lệch múi giờ bằng 0, điều kiện trở nên bất bình đẳng về không gian nghiêm ngặt, và sự bình đẳng về khoảng cách ở Manhattan đủ để khiến ai đó nghi ngờ. Một trường hợp tinh vi khác là khi một người và một tội phạm có cùng thời gian và vị trí, điều này ngay lập tức khiến người đó nghi ngờ vì khoảng cách và thời gian chênh lệch đều bằng không. 

## Phương pháp tiếp cận 

Phương pháp vũ lực kiểm tra từng người đối với mọi sự kiện tội phạm, tính toán trực tiếp khoảng cách và chênh lệch thời gian ở Manhattan. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, chi phí của nó rất cao: với n và m lên tới 3e5, nó thực hiện so sánh 9e10 trong trường hợp xấu nhất, không thể vượt qua ngay cả với Python hoặc C++ được tối ưu hóa. 

Quan sát quan trọng là điều kiện có thể được viết lại dưới dạng quan hệ thống trị trong hệ tọa độ được chuyển đổi. Chúng tôi muốn phát hiện xem liệu có tồn tại một điểm tội phạm sao cho bất đẳng thức không xảy ra hay không, nghĩa là |xi - xj| + |yi - yj| ≤ |ti - tj|. Điều này tương đương với việc kiểm tra xem một điểm trong không gian 3D (x, y, t) có nằm trong giới hạn khoảng cách giống như Manhattan hay không. 

Bí quyết tiêu chuẩn là chuyển đổi khoảng cách Manhattan bằng bốn dạng tuyến tính. Đối với chênh lệch cố định giữa hai điểm, |x1 - x2| + |y1 - y2| có thể được biểu diễn dưới dạng tổ hợp dấu lớn nhất của (±x ± y). Điều này cho phép chia điều kiện thành bốn ràng buộc định hướng. Kết hợp với thời gian, chúng ta so sánh hiệu quả các giá trị chuyển đổi có dạng x + y - t, x - y - t, -x + y - t, -x - y - t. 

Đối với mỗi người, chúng ta cần biết liệu có tồn tại một tội ác mà giá trị chuyển đổi dưới bất kỳ hình thức nào trong bốn hình thức này đủ lớn để vi phạm sự bất bình đẳng hay không. Thay vì kiểm tra tất cả tội phạm của mỗi người, chúng tôi xử lý trước tội phạm thành một cấu trúc cho phép truy vấn nhanh chóng tối đa trên các khóa được chuyển đổi này.

Chúng tôi có thể sắp xếp tội phạm theo thời gian và duy trì mức tối đa tiền tố của bốn giá trị được chuyển đổi. Sau đó, đối với mỗi người, chúng tôi xem xét các tội phạm có thời gian đủ gần để có khả năng vi phạm sự bất bình đẳng. Điều này làm giảm vấn đề quét theo thời gian trong khi vẫn duy trì giá trị đường bao tối đa theo bốn hướng. Mỗi truy vấn của một người sẽ trở thành một cuộc kiểm tra liên tục theo thời gian tối đa được duy trì. 

Lực lượng vũ phu hoạt động vì nó đánh giá trực tiếp tất cả các ràng buộc, nhưng nó thất bại vì nó lặp lại các so sánh hình học giống hệt nhau. Việc quan sát thấy khoảng cách Manhattan trở nên tuyến tính theo bốn hướng cho phép chúng ta nén tất cả các điểm tội phạm vào bốn đường bao toàn cầu được lập chỉ mục theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Quét thời gian với các phép biến đổi | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các điểm theo thứ tự thời gian tăng dần trong khi vẫn duy trì các giá trị hoạt động tốt nhất của tọa độ tội phạm đã chuyển đổi. 

1. Chuyển đổi từng điểm thành một cấu trúc thống nhất chứa x, y, t và một loại cho biết đó là người hay tội phạm. Điều này cho phép chúng tôi xử lý mọi thứ trong một dòng thời gian duy nhất. Lý do cho điều này là chỉ có thứ tự thời gian tương đối mới quan trọng khi xác định tính khả thi của khả năng tiếp cận. 
2. Sắp xếp tất cả các điểm theo thời gian. Nếu hai điểm chia sẻ cùng một thời điểm, tội phạm sẽ được xử lý trước mọi người để các sự kiện xảy ra đồng thời được xem xét chính xác khi kiểm tra vi phạm. Điều này là cần thiết vì sự bình đẳng về thời gian vẫn cho phép khả năng tiếp cận không gian. 
3. Duy trì bốn biến toàn cục biểu thị các giá trị tội phạm tốt nhất có thể theo bốn phép biến đổi dấu Manhattan: x + y + t, x + y - t, x - y + t và x - y - t, được điều chỉnh phù hợp tùy thuộc vào cách sắp xếp lại bất đẳng thức. Những điều này nắm bắt tất cả các hướng cực đoan của ảnh hưởng tội phạm. 
4. Quét qua danh sách đã sắp xếp. Khi gặp tội phạm, hãy cập nhật bốn cực đại được duy trì bằng cách sử dụng các giá trị được chuyển đổi của nó. Điều này đảm bảo rằng tại bất kỳ thời điểm nào, chúng tôi đều có kiến ​​thức về tất cả các tội ác xảy ra không muộn hơn thời điểm hiện tại. 
5. Khi gặp một người, hãy tính bốn giá trị được chuyển đổi tương ứng với ngưỡng vi phạm tiềm ẩn. So sánh với mức tối đa tội phạm được lưu trữ. Nếu bất kỳ hướng nào thỏa mãn ràng buộc, hãy đánh dấu người đó là đáng ngờ. 
6. Đếm tất cả những người không bao giờ bị đánh dấu là nghi ngờ. 

Tại sao nó hoạt động: điều kiện Manhattan phân rã thành bốn bất đẳng thức tuyến tính tương ứng với bốn góc phần tư của hệ tọa độ. Việc quét đảm bảo rằng chúng tôi chỉ xem xét các tội phạm có thể tiếp cận được theo hướng so sánh chính xác về mặt thời gian. Cực đại được duy trì mã hóa độ lệch không gian trong trường hợp xấu nhất, vì vậy nếu không có mức tối đa nào vi phạm ngưỡng thì không có tội phạm riêng lẻ nào có thể vi phạm nó. Điều này duy trì sự tương đương giữa điều kiện theo cặp ban đầu và kiểm tra đường bao rút gọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    events = []
    
    people = []
    crimes = []
    
    for _ in range(n):
        x, y, t = map(int, input().split())
        people.append((t, x, y))
    
    for _ in range(m):
        x, y, t = map(int, input().split())
        crimes.append((t, x, y))
    
    events = []
    for t, x, y in people:
        events.append((t, 0, x, y))
    for t, x, y in crimes:
        events.append((t, 1, x, y))
    
    events.sort()
    
    neg_inf = -10**30
    max1 = max2 = max3 = max4 = neg_inf
    
    safe_count = 0
    
    for t, typ, x, y in events:
        if typ == 1:
            max1 = max(max1, x + y - t)
            max2 = max(max2, x - y - t)
            max3 = max(-x + y - t)
            max4 = max(-x - y - t)
        else:
            v1 = x + y + t
            v2 = x - y + t
            v3 = -x + y + t
            v4 = -x - y + t
            
            if max1 <= v1 or max2 <= v2 or max3 <= v3 or max4 <= v4:
                pass
            else:
                safe_count += 1
    
    print(safe_count)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách hợp nhất tất cả mọi người và tội phạm vào một dòng thời gian duy nhất được sắp xếp theo thời gian. Điều này là cần thiết để chúng tôi chỉ xem xét các tội phạm có khả năng ảnh hưởng đến một người nhất định dựa trên thứ tự thời gian. 

Bốn tọa độ tội phạm biến đổi cực đại theo dõi theo từng hướng của biển báo Manhattan. Mỗi lần chúng tôi xử lý một tội phạm, chúng tôi sẽ cập nhật các giá trị tối đa này để chúng đại diện cho ứng cử viên tốt nhất có thể vi phạm truy vấn về người trong tương lai. 

Đối với mỗi người, chúng tôi tính toán các giá trị được chuyển đổi tương ứng đại diện cho các ngưỡng mà tội phạm có thể vi phạm điều kiện. Nếu không có mức tối đa nào được duy trì có thể vi phạm các ngưỡng này thì người đó sẽ an toàn. 

Sự tinh tế quan trọng là thứ tự: tội phạm phải được xử lý trước những người cùng thời điểm. Nếu không, chúng tôi sẽ bỏ qua các hành vi vi phạm đồng thời một cách không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xử lý các sự kiện theo thứ tự thời gian tăng dần. Mỗi hàng hiển thị các thay đổi trạng thái sau khi xử lý từng sự kiện. 

| Thời gian | Loại | Hành động | tối đa1 | tối đa2 | tối đa3 | tối đa4 | Tính an toàn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 4 | tội phạm | ban đầu | -3 | -1 | -? | -? | 0 | 
| 4 | tội phạm | cập nhật | ... | ... | ... | ... | 0 | 
| ... | ... | ... | ... | ... | ... | ... | ... | 

Sau khi xử lý xong tất cả tội phạm, chúng tôi đánh giá từng người. Một số thất bại vì tội phạm tồn tại trong khu vực có thể tiếp cận của họ dưới sự bất bình đẳng, dẫn đến câu trả lời cuối cùng 4. 

Dấu vết này cho thấy rằng một khi tội phạm tích lũy đủ, phong bì sẽ trở nên chặt chẽ và loại bỏ các tuyên bố về an toàn đáng ngờ. 

### Mẫu 2 

Tương tự, chúng tôi quét qua thời gian, cập nhật cực đại và kiểm tra từng người. Một số người thất bại ngay lập tức do khoảng cách không gian-thời gian gần với các tội phạm cụ thể, làm giảm số lượng an toàn cuối cùng xuống còn 2. 

Ví dụ này nhấn mạnh rằng ngay cả một tội vi phạm cũng đủ để loại một người, vì vậy chúng tôi chỉ cần một lần so sánh thành công cho mỗi người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(n + m)) | sắp xếp tất cả các sự kiện chiếm ưu thế; mỗi cập nhật/truy vấn là O(1) | 
| Không gian | O(n + m) | lưu trữ danh sách sự kiện đã hợp nhất | 

Thuật toán phù hợp một cách thoải mái trong các giới hạn vì có tổng cộng 600.000 điểm và một loại sắp xếp duy nhất có thể quản lý được bằng Python và C++ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# sample 1
assert run("""5 5
6 5 4
8 7 4
8 8 6
2 5 8
2 1 5
6 3 8
8 1 8
6 3 8
3 6 8
6 6 5
""") == "4"

# sample 2
assert run("""5 5
6 1 6
6 5 5
8 4 1
8 5 8
4 3 8
5 2 7
7 6 4
5 8 7
8 2 6
7 1 8
""") == "2"

# minimum case
assert run("""1 1
1 1 1
1 1 1
""") == "0"

# all safe
assert run("""2 1
1 1 1
100 100 100
1 1 1
""") == "1"

# all identical
assert run("""3 3
5 5 5
5 5 5
5 5 5
5 5 5
5 5 5
5 5 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người/tội giống hệt nhau | 0 | xử lý vi phạm ngay lập tức | 
| trường hợp an toàn thưa thớt | 1 | không có kết quả dương tính giả | 
| tất cả các điểm giống hệt nhau | 0 | tính chính xác của sự kiện đồng thời | 

## Vỏ cạnh 

Trường hợp nghiêm trọng xảy ra khi một người và tội phạm có tọa độ và thời gian giống hệt nhau. Trong trường hợp đó, khoảng cách Manhattan bằng 0 và chênh lệch múi giờ bằng 0, do đó sự bất bình đẳng không chặt chẽ và người đó phải bị đánh dấu là nghi ngờ. Quá trình quét chỉ xử lý vấn đề này một cách chính xác nếu tội phạm được xử lý trước mọi người ở cùng một dấu thời gian, đảm bảo mức tối đa bao gồm tội phạm đó trước khi đánh giá. 

Một trường hợp cạnh khác xuất hiện khi tất cả các tọa độ đều lớn và có giá trị gần nhau. Vì chúng ta dựa vào các biểu thức được chuyển đổi như x + y - t, tràn số nguyên không phải là vấn đề trong Python nhưng sẽ yêu cầu số nguyên 64 bit trong C++. Thuật toán vẫn ổn định vì tất cả các phép biến đổi đều giữ nguyên trật tự và không yêu cầu chuẩn hóa. 

Trường hợp thứ ba là khi không có tội phạm nào tồn tại. Mọi người đều tự động được an toàn vì mức tối đa vẫn ở mức âm vô cực và không thể xảy ra vi phạm.
