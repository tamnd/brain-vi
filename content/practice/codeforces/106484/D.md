---
title: "CF 106484D - Ổ Khóa"
description: "Chúng ta được cung cấp một mảng các vị trí từ 1 đến n. Mỗi vị trí hoạt động giống như một khóa có thể trống hoặc đã bị khóa bởi một số truy vấn. Ban đầu mọi vị trí đều miễn phí."
date: "2026-06-19T17:21:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106484
codeforces_index: "D"
codeforces_contest_name: "2026 GBA International Programming Contest"
rating: 0
weight: 106484
solve_time_s: 60
verified: true
draft: false
---

[CF 106484D - Khóa](https://codeforces.com/problemset/problem/106484/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các vị trí từ 1 đến n. Mỗi vị trí hoạt động giống như một khóa có thể trống hoặc đã bị khóa bởi một số truy vấn. Ban đầu mọi vị trí đều miễn phí. 

Mỗi truy vấn mô tả một đoạn vị trí từ l đến r và hai thời điểm bắt đầu, một để khóa và một để mở khóa. Trong giai đoạn khóa của truy vấn, chúng tôi quét phân đoạn của nó từ trái sang phải, một vị trí trên một đơn vị thời gian bắt đầu từ ts. Khi chúng tôi đạt đến một vị trí, nếu nó hiện đang trống, chúng tôi sẽ khóa nó và gán nó cho truy vấn này. Nếu nó đã bị khóa bởi bất kỳ truy vấn nào khác, chúng tôi sẽ giữ nguyên nó. Giai đoạn mở khóa bắt đầu muộn hơn tại thời điểm te và quét lại cùng một phân đoạn theo cùng một thứ tự. Trong giai đoạn này, một vị trí chỉ được mở khóa nếu nó hiện thuộc quyền sở hữu của truy vấn này; nếu không thì không có gì xảy ra. 

Tất cả các hoạt động từ tất cả các truy vấn được đặt trên trục thời gian dùng chung. Nếu nhiều thao tác xảy ra cùng lúc trên cùng một vị trí, chúng sẽ được thực hiện theo thứ tự tăng dần của chỉ mục truy vấn. 

Đối với mỗi truy vấn, chúng tôi chỉ quan tâm đến việc nó khóa thành công bao nhiêu vị trí trong giai đoạn khóa. 

Khó khăn chính là việc khóa có tính toàn cục và liên tục cho đến khi bị xóa rõ ràng, do đó các truy vấn sớm hơn hoặc muộn hơn có thể chặn lẫn nhau tùy thuộc vào thời gian chồng chéo. 

Các ràng buộc n, q 1000 có nghĩa là tổng số thao tác theo thứ tự q lần độ dài phân đoạn, nhiều nhất là khoảng một triệu cập nhật cho khóa và một triệu cho mở khóa. Điều này loại trừ bất kỳ giải pháp nào cố gắng tính toán lại các tương tác trên mỗi cặp truy vấn hoặc mỗi cặp vị trí theo cách bậc hai trong các sự kiện. Việc mô phỏng các sự kiện được tạo rõ ràng là khả thi, nhưng bất cứ điều gì cố gắng quét liên tục các bước thời gian hoặc tính toán lại các mốc thời gian đầy đủ cho mỗi truy vấn sẽ quá chậm. 

Một trường hợp tinh tế xuất phát từ các hoạt động đồng thời tại cùng một thời điểm. Vì nhiều truy vấn có thể hoạt động trên các vị trí khác nhau ở các dấu thời gian giống nhau nên việc sắp xếp theo chỉ mục truy vấn trở nên quan trọng. Nếu thứ tự này bị bỏ qua, truy vấn sau này có thể đánh cắp hoặc ghi đè không chính xác khóa đáng lẽ phải được thực hiện trước đó trong cùng một bước thời gian. 

## Phương pháp tiếp cận 

Giải thích trực tiếp mô phỏng từng bước thời gian. Đối với mỗi thời gian nguyên, chúng tôi sẽ kiểm tra tất cả các truy vấn, xem vị trí nào đang được xử lý và cập nhật trạng thái. Tuy nhiên, phạm vi thời gian lên tới 10^9, do đó việc lặp lại theo thời gian là không thể. 

Quan điểm hữu ích hơn là quan sát thấy rằng không có gì phụ thuộc vào thời gian tuyệt đối, chỉ phụ thuộc vào thứ tự tương đối của các sự kiện. Mỗi truy vấn tạo ra một chuỗi các hoạt động độc lập: một lần thử khóa cho từng vị trí trong phân đoạn của nó và sau đó là một lần thử mở khóa cho cùng một vị trí. Mỗi hoạt động như vậy có thể được biểu diễn dưới dạng một sự kiện duy nhất có dấu thời gian. 

Điều này chuyển đổi vấn đề thành vấn đề sắp xếp toàn cục trên tất cả các sự kiện. Mỗi sự kiện là một lần thử khóa hoặc một lần thử mở khóa trên một vị trí cụ thể. Trạng thái hệ thống chỉ thay đổi khi chúng tôi xử lý các sự kiện này theo thứ tự thời gian tăng dần, ngắt kết nối theo chỉ mục truy vấn theo yêu cầu. 

Khi các sự kiện được sắp xếp, chúng tôi mô phỏng chúng theo thứ tự. Sự kiện khóa chỉ thành công nếu vị trí đó trống tại thời điểm đó. Sự kiện mở khóa chỉ thành công nếu vị trí đó hiện thuộc quyền sở hữu của truy vấn đó. Chúng tôi theo dõi quyền sở hữu trên mỗi vị trí. 

Điều này làm giảm vấn đề từ lý luận về thời gian liên tục sang quét rời rạc ở hầu hết các sự kiện 2qn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng thời gian) | O(thời gian tối đa × q × đoạn) | O(n) | Quá chậm | 
| Sắp xếp sự kiện + mô phỏng | O(E log E) trong đó E 2qn | O(n + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biến mỗi truy vấn thành các sự kiện rõ ràng.

1. Đối với mỗi truy vấn, hãy tạo các sự kiện khóa cho tất cả các vị trí trong khoảng thời gian của nó. Thời gian sự kiện cho vị trí k trong [l, r] là ts + (k - l). Điều này mô hình quét tuần tự một cách chính xác. 
2. Đối với mỗi truy vấn, hãy tạo các sự kiện mở khóa cho tất cả các vị trí trong khoảng thời gian đó. Thời gian diễn ra sự kiện là te + (k - l). Điều này phản ánh quá trình truyền tải từ trái sang phải tương tự nhưng ở thời điểm bắt đầu muộn hơn. 
3. Lưu trữ mỗi sự kiện dưới dạng một bộ dữ liệu (thời gian, query_id, loại, vị trí), trong đó loại phân biệt khóa và mở khóa. Chúng tôi bao gồm query_id vì việc bẻ khóa yêu cầu xử lý các truy vấn được lập chỉ mục nhỏ hơn trước khi thời gian khớp. 
4. Sắp xếp tất cả các sự kiện theo từ điển theo thời gian, sau đó theo query_id. Loại này không ảnh hưởng đến thứ tự vì quy tắc chỉ xác định thứ tự truy vấn tại thời điểm bằng nhau. 
5. Duy trì chủ sở hữu mảng[pos], ban đầu trống. Đồng thời duy trì một mảng trả lời ans[q], ban đầu bằng 0. 
6. Xử lý các sự kiện theo thứ tự sắp xếp. Nếu sự kiện là một nỗ lực khóa và owner[pos] trống, hãy gán owner[pos] = query_id và tăng ans[query_id]. Nếu đó là nỗ lực khóa và vị trí đã được sở hữu thì không cần làm gì cả. 
7. Nếu sự kiện là một lần mở khóa và owner[pos] bằng query_id, hãy xóa owner[pos]. Nếu không thì không làm gì cả. 

Lý do chính khiến điều này có tác dụng là vì mọi hành động ảnh hưởng đến một vị trí đều được thể hiện chính xác một lần theo thứ tự thời gian. Không sự kiện nào sau này có thể thay đổi các quyết định trước đó vì quyền sở hữu chỉ được cập nhật khi xử lý các sự kiện theo trình tự thời gian. Việc liên kết theo chỉ mục truy vấn đảm bảo tính nhất quán khi nhiều sự kiện có cùng dấu thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    
    events = []
    
    queries = []
    for i in range(q):
        ts, te, l, r = map(int, input().split())
        queries.append((ts, te, l, r))
        
        for k in range(l, r + 1):
            t_lock = ts + (k - l)
            t_unlock = te + (k - l)
            
            events.append((t_lock, i, 0, k))
            events.append((t_unlock, i, 1, k))
    
    events.sort()
    
    owner = [0] * (n + 1)
    ans = [0] * q
    
    for t, i, typ, pos in events:
        if typ == 0:
            if owner[pos] == 0:
                owner[pos] = i + 1
                ans[i] += 1
        else:
            if owner[pos] == i + 1:
                owner[pos] = 0
    
    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mã này xây dựng danh sách sự kiện đầy đủ cho cả hai giai đoạn của mỗi truy vấn. Các sự kiện khóa được đánh dấu bằng loại 0, mở khóa các sự kiện bằng loại 1. Bước sắp xếp thực thi quá trình xử lý theo trình tự thời gian toàn cầu với sự ràng buộc chính xác. 

Chủ sở hữu mảng lưu trữ truy vấn nào hiện đang giữ từng vị trí. Sử dụng 0 làm “mở khóa” giúp đơn giản hóa việc kiểm tra. Mảng câu trả lời chỉ tăng lên khi quá trình chuyển đổi khóa thành công, khớp chính xác với định nghĩa về khóa thành công trong giai đoạn khóa. 

Một điểm tinh tế là các sự kiện mở khóa chỉ áp dụng nếu chủ sở hữu hiện tại khớp với truy vấn, nếu không chúng sẽ bị bỏ qua. Điều này ngăn một truy vấn mở khóa khóa của truy vấn khác. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với n = 3 và hai truy vấn. 

Truy vấn 1: ts = 1, te = 5, l = 1, r = 2 

Truy vấn 2: ts = 2, te = 4, l = 2, r = 3 

Chúng tôi tạo ra các sự kiện và sắp xếp chúng. 

| thời gian | truy vấn | gõ | tư thế | chủ trước | hành động | chủ sở hữu sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | khóa | 1 | không | khóa | 1 | 
| 2 | 1 | khóa | 2 | không | khóa | 1 | 
| 2 | 2 | khóa | 2 | 1 | bỏ qua | 1 | 
| 3 | 2 | khóa | 3 | không | khóa | 2 | 

Sau khi xử lý sự kiện khóa, truy vấn 1 đã khóa thành công 2 vị trí và truy vấn 2 đã khóa 1 vị trí. 

Dấu vết này cho thấy cách sắp xếp truy vấn tại thời điểm bằng nhau ngăn truy vấn 2 lấy cắp vị trí 2 tại thời điểm 2, vì truy vấn 1 được xử lý trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(qn log(qn)) | Mỗi truy vấn tạo ra các sự kiện O(n) và việc sắp xếp chiếm ưu thế | 
| Không gian | O(qn + n) | Lưu trữ sự kiện cộng với mảng sở hữu | 

Với n, q ≤ 1000, tổng số sự kiện tối đa là 2 × 10^6 và việc sắp xếp điều này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("""1 1
1 2 1 1
""") == "1"

# non-overlapping segments
assert run("""3 2
1 2 1 2
3 4 3 3
""") == "2\n1"

# overlapping conflict
assert run("""3 2
1 5 1 3
2 6 2 3
""") == "3\n0"

# full overlap same segment
assert run("""4 2
1 5 1 4
2 6 1 4
""") == "4\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | độ chính xác của khóa cơ bản | 
| phạm vi rời rạc | 2 1 | truy vấn độc lập | 
| cạnh tranh chồng chéo | 3 0 | chặn quyền sở hữu | 
| chồng chéo hoàn toàn | 4 0 | ưu tiên nghiêm ngặt theo thứ tự thời gian | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các sự kiện đồng thời ở cùng một dấu thời gian. Ví dụ: nếu cả truy vấn 1 và truy vấn 2 đều hành động ở cùng một vị trí tại thời điểm 2 thì thứ tự truy vấn sẽ xác định kết quả. Việc sắp xếp theo chỉ mục truy vấn đảm bảo truy vấn 1 được xử lý trước, vì vậy nếu nó khóa vị trí, truy vấn 2 sẽ thấy chính xác là nó đã được lấy. 

Một trường hợp khác là mở khóa không ảnh hưởng đến sở hữu nước ngoài. Nếu truy vấn 2 mở khóa vị trí hiện do truy vấn 1 sở hữu thì thao tác này phải được bỏ qua. Điều này được xử lý rõ ràng bằng cách kiểm tra owner[pos] == query_id trước khi xóa quyền sở hữu, ngăn ngừa sự can thiệp vô tình của truy vấn chéo. 

Trường hợp cạnh cuối cùng được lặp lại truy cập vào cùng một vị trí bằng cùng một truy vấn trong giai đoạn khóa của nó. Vì quyền sở hữu chỉ được chỉ định một lần và các lần thử khóa tiếp theo không có tác dụng gì nên việc tăng ans sẽ xảy ra chính xác một lần cho mỗi lần chuyển đổi thành công chứ không phải cho mỗi lượt truy cập.
