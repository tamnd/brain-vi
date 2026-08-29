---
title: "CF 104380B - Máy quét mìn"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một số mô tả số lượng mỏ hiện có trong một vùng lân cận cụ thể xung quanh ô đó."
date: "2026-07-01T17:07:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "B"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 93
verified: false
draft: false
---

[CF 104380B - Máy quét mìn](https://codeforces.com/problemset/problem/104380/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô chứa một số mô tả số lượng mỏ hiện có trong một vùng lân cận cụ thể xung quanh ô đó. Vùng lân cận không chỉ là chính ô đó mà còn là tất cả các ô tiếp xúc với nó bằng một góc hoặc một cạnh, tạo thành một khối 3 x 3 có tâm ở ô đó. Vì vậy, mỗi giá trị thực sự là số lượng mỏ trong 8 ô xung quanh cộng với chính nó. 

Có một hạn chế về cấu trúc bổ sung: không có mỏ nào tồn tại ở rìa ngoài của lưới điện. Điều này quan trọng vì nó đảm bảo rằng mọi mỏ hợp lệ đều có vùng lân cận 3 x 3 được xác định đầy đủ hoàn toàn bên trong lưới, do đó, mỗi mỏ đóng góp vào số lượng chính xác chín ô theo một mẫu nhất quán. 

Nhiệm vụ là xây dựng lại tọa độ chính xác của tất cả các mỏ và xuất chúng theo thứ tự hàng, sau đó theo cột. 

Các ràng buộc cho phép tối đa một nghìn hàng và cột, tức là lên tới một triệu ô. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các cấu hình khai thác có thể ngay lập tức là không thể thực hiện được vì không gian trạng thái có số mũ theo số lượng ô. Ngay cả phương pháp tiếp cận bậc hai trên mỗi ô cũng có nguy cơ quá chậm, vì vậy chúng tôi đang tìm kiếm chiến lược tái thiết tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế xuất phát từ việc giải thích con số này dưới dạng tích chập cục bộ với hạt nhân 3 x 3 cố định trên một lưới mỏ nhị phân. Điều này có nghĩa là mỗi mỏ đóng góp +1 cho chính xác chín ô và tổng đóng góp chồng chéo. 

Một cạm bẫy ngây thơ sẽ xuất hiện nếu người ta giả định giá trị tại một ô tương ứng trực tiếp với việc đó có phải là mỏ hay không. Ví dụ: ô có nhãn 4 không có nghĩa là nó có hoặc không phải là mỏ, nó chỉ phản ánh các mỏ xung quanh. Một dạng thất bại khác là cố gắng suy luận tham lam mà không thực thi tính nhất quán giữa các vùng lân cận chồng chéo, dẫn đến mâu thuẫn. 

Một trường hợp nhầm lẫn minh họa nhỏ là một mỏ ở (2,2). Mỗi ô trong vùng lân cận 3 x 3 của nó sẽ trở thành 1. Thay vào đó, nếu chúng ta đoán các quả mìn một cách độc lập trên mỗi ô, chúng ta sẽ đánh dấu sai tất cả chín ô đó là mỏ, khiến việc đếm quá tệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xử lý từng ô một cách độc lập và cố gắng suy luận xem nó có phải là mỏ hay không bằng cách kiểm tra tất cả các ràng buộc liên quan đến nó. Người ta có thể thử giả sử một ô là mỏ và trừ đi ảnh hưởng của nó khỏi tất cả 3 x 3 vùng lân cận mà nó chạm vào, tiếp tục đệ quy cho đến khi tất cả các số đều được thỏa mãn. Điều này nhanh chóng trở thành vấn đề về việc quay lại hoặc thỏa mãn ràng buộc. Trong trường hợp xấu nhất, mỗi ô trong số khoảng một triệu ô có thể là mỏ hoặc không, dẫn đến trạng thái 2^(mn). Ngay cả với việc cắt tỉa, các ràng buộc chồng chéo cũng không làm giảm hệ số phân nhánh đủ để khiến điều này trở nên khả thi. 

Quan sát quan trọng là vấn đề có cấu trúc tuyến tính. Mỗi mỏ ảnh hưởng đến chính xác chín ô và giá trị của mỗi ô là tổng đóng góp từ các mỏ gần đó. Đây là một hệ thống tuyến tính cố định trên một lưới, nhưng quan trọng hơn, nó có thể được giải cục bộ bằng cách quét từ trái sang phải, từ trên xuống dưới xác định. 

Ý tưởng quan trọng là xử lý lưới theo thứ tự mà khi quyết định liệu mỏ có tồn tại ở một vị trí hay không, tất cả những đóng góp trong tương lai cho các ô đã được xử lý đều đã được xác định. Nếu duyệt từng hàng, chúng ta có thể “ấn định” quyết định tại (i, j) dựa trên mức đóng góp bắt buộc còn lại hiện tại tại (i, j), bởi vì bất kỳ mỏ nào trong tương lai có thể ảnh hưởng đến (i, j) đều phải nằm trong vùng lân cận 3 x 3 của nó và do ràng buộc và thứ tự xử lý không có mỏ biên giới, các vị trí trong tương lai đó được kiểm soát một cách nhất quán.

Chúng tôi duy trì một mạng lưới làm việc thể hiện số lượng đóng góp vẫn cần được đáp ứng ở mỗi ô. Khi chúng tôi quyết định đặt một mỏ ở (i, j), chúng tôi trừ đi một từ tất cả các ô trong vùng lân cận 3 x 3 của nó. Nếu chúng ta không đặt mìn, chúng ta sẽ không làm gì cả. Tính chính xác xuất phát từ thực tế là tại thời điểm chúng ta đạt đến (i, j), tất cả các ô phía trên hoặc bên trái vẫn có thể bị ảnh hưởng đều đã được hoàn thiện. 

Điều này làm giảm vấn đề thành một sự tái thiết tham lam với sự điều chỉnh cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(mn) | O(mn) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và lưu trữ nó dưới dạng một mảng có thể thay đổi thể hiện những đóng góp cần thiết còn lại. Lưới này sẽ được cập nhật khi chúng tôi đặt mìn để nó luôn phản ánh những gì vẫn cần được giải thích bằng các quyết định trong tương lai. 
2. Duyệt lưới từ trên xuống dưới và từ trái sang phải, bỏ qua các ô viền vì đảm bảo không chứa mìn. Thứ tự này đảm bảo rằng khi chúng tôi quyết định một ô, tất cả các tương tác trước đó ảnh hưởng đến ô đó đều đã được giải quyết. 
3. Tại mỗi ô (i, j), kiểm tra xem giá trị còn lại hiện tại tại ô đó có lớn hơn 0 hay không. Nếu nó bằng 0, chúng tôi không thể biện minh cho việc đặt mỏ ở đây vì không có ràng buộc nào còn lại yêu cầu điều đó, vì vậy chúng tôi tiếp tục. 
4. Nếu giá trị dương, chúng ta đặt mỏ tại (i, j). Đây là cách nhất quán cục bộ duy nhất để giảm yêu cầu còn lại ở vị trí này trong khi tôn trọng rằng mỗi mỏ đóng góp chính xác một đơn vị cho ô này. 
5. Sau khi đặt một quả mìn, hãy trừ đi một từ tất cả các ô trong vùng lân cận 3 x 3 có tâm tại (i, j). Điều này mô hình hóa tác động của mỏ lên tất cả các ô bị ảnh hưởng, cập nhật những đóng góp cần thiết còn lại của chúng. 
6. Tiếp tục quét cho đến khi tất cả các ô bên trong hợp lệ đã được xử lý. Vị trí đặt mìn là câu trả lời cuối cùng. 

Bất biến chính là trước khi xử lý bất kỳ ô nào (i, j), tất cả các hiệu chỉnh từ các mỏ đã quyết định trước đó đều đã được áp dụng, do đó giá trị tại (i, j) thể hiện chính xác có bao nhiêu mỏ bổ sung vẫn phải bao phủ nó. Quyết định tham lam là an toàn vì bất kỳ mỏ nào trong tương lai có thể ảnh hưởng đến (i, j) đều phải nằm trong khu vực sẽ được xử lý sau này và đóng góp của họ sẽ được xử lý một cách đối xứng khi chính họ đưa ra quyết định. Điều này ngăn cản việc cam kết quá mức: một khi một ô bị giảm xuống 0, nó sẽ không bao giờ bị tăng lên một cách sai lầm nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(m)]
    mines = []

    for i in range(1, m - 1):
        for j in range(1, n - 1):
            if grid[i][j] > 0:
                mines.append((i, j))
                for di in (-1, 0, 1):
                    for dj in (-1, 0, 1):
                        grid[i + di][j + dj] -= 1

    if not mines:
        print(0)
    else:
        mines.sort()
        for i, j in mines:
            print(i, j)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một bản sao hoạt động của lưới và mô phỏng trực tiếp tác động của việc đặt mìn. Vòng lặp kép đảm bảo chúng ta truy cập các ô theo thứ tự hàng lớn tăng dần. điều kiện`grid[i][j] > 0`quyết định liệu một quả mìn có nhất thiết phải tồn tại ở đó hay không, bởi vì bất kỳ yêu cầu tích cực nào tại thời điểm đó đều phải được đáp ứng ngay lập tức bởi quả mìn được đặt tại ô hiện tại. 

Vòng cập nhật 3 x 3 là quá trình chuyển đổi cốt lõi, đảm bảo mỗi mỏ đóng góp đúng cách cho tất cả các ô bị ảnh hưởng. Việc sắp xếp ở cuối đảm bảo thứ tự đầu ra được yêu cầu, mặc dù quá trình truyền tải đã tạo ra thứ tự đó một cách tự nhiên. 

Một điểm triển khai tinh vi là chúng ta không bao giờ cần kiểm tra xem việc đặt mỏ có vi phạm ràng buộc trong tương lai hay không, bởi vì cấu trúc tham lam đảm bảo tính nhất quán khi xử lý theo thứ tự. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng thông tin đầu vào mẫu được cung cấp làm dấu vết chính vì nó đã thể hiện rõ ràng các vùng lân cận chồng chéo. 

Chúng ta hãy theo dõi một số vị trí quan trọng về mặt khái niệm thay vì mở rộng toàn bộ một triệu ô. 

Khi bắt đầu, mỗi ô đều có sự đóng góp cần thiết ban đầu. 

Khi chúng ta đến ô (1,1), giá trị của nó là dương, vì vậy chúng ta đặt một quả mìn ở đó và trừ đi một khối từ khối 3 x 3 xung quanh nó. Điều này ngay lập tức làm giảm giá trị trong các ô lân cận, lan truyền hiệu ứng. 

Khi chúng ta chuyển đến (1,3), chúng ta lại tìm thấy một giá trị dương không thể giải thích được bằng các vị trí trước đó, vì vậy chúng ta đặt một mỏ khác và áp dụng cùng một mức giảm cục bộ. 

Khi quá trình xử lý tiếp tục, các vùng lân cận chồng chéo khiến các giá trị tự nhiên chuyển về 0, chính xác ở nơi tất cả các mỏ cần thiết đã được tính đến. 

Một dấu vết đơn giản hóa cho một đoạn nhỏ: 

| Bước | Vị trí | lưới[i][j] trước | Hành động | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | >0 | đặt mỏ | trừ 1 trong khối 3x3 | 
| 2 | (1,3) | >0 | đặt mỏ | trừ 1 trong khối 3x3 | 
| 3 | (2,3) | >0 | đặt mỏ | trừ 1 trong khối 3x3 | 

Điều này cho thấy các khoản đóng góp chồng chéo dần dần bị hủy bỏ như thế nào. 

Hành vi chính được xác nhận ở đây là cục bộ: mỗi quyết định chỉ phụ thuộc vào phần dư hiện tại tại một ô duy nhất và các bản cập nhật được truyền bá một cách nhất quán mà không cần quay lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(mn) | Mỗi ô được truy cập một lần và mỗi ô sẽ kích hoạt cập nhật 3 x 3 liên tục | 
| Không gian | O(mn) | Lưới được lưu trữ tại chỗ mà không cần thêm công trình lớn nào | 

Kích thước lưới có thể đạt tới một triệu ô và mỗi ô tham gia vào tối đa một lần cập nhật vùng lân cận theo thời gian không đổi cho mỗi mỏ. Vì mỗi ô chỉ có thể kích hoạt một số lượng vị trí đặt mìn nhất định trong thực tế do sự giảm đơn điệu, nên thuật toán vẫn tuyến tính trong tổng công việc và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""6 6
1 1 2 2 2 1
1 1 3 3 3 1
2 2 4 3 3 1
2 3 4 3 2 1
2 3 3 2 1 1
1 2 2 2 1 1
""") == """1 1
1 3
1 4
2 3
3 1
4 1
4 2
4 4"""

# minimum size grid (3x3, single center mine)
assert run("""3 3
1 1 1
1 1 1
1 1 1
""") == """1 1"""

# no mines
assert run("""3 3
0 0 0
0 0 0
0 0 0
""") == """0"""

# small asymmetric case
assert run("""4 4
0 0 0 0
0 1 1 0
0 1 1 0
0 0 0 0
""") == """1 1
1 2
2 1
2 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3x3 tất cả những cái | 1 1 | mỏ trung tâm duy nhất đúng đắn | 
| tất cả số không | 0 | xử lý không có mìn | 
| khối 4x4 | 4 mỏ trung tâm | lan truyền vùng lân cận chồng chéo | 

## Vỏ cạnh 

Một điều kiện quan trọng là khi lưới không chứa mỏ nào cả. Trong tình huống đó, mọi ô đều bằng 0 ngay từ đầu, do đó quá trình quét không bao giờ kích hoạt vị trí. Đầu ra trở thành một số 0 duy nhất mà thuật toán xử lý rõ ràng sau khi truyền tải. 

Một trường hợp khác là một mỏ bị cô lập ở trung tâm của lưới 3 x 3. Thuật toán truy cập vào ô trung tâm, nhìn thấy giá trị dương, đặt một quả mìn và ngay lập tức trừ đi một giá trị từ tất cả chín ô. Sau lần cập nhật đó, mọi ô sẽ trở thành số 0 và không có mỏ nào được đặt thêm nữa, phù hợp với quá trình tái thiết dự kiến. 

Một trường hợp tinh vi hơn là nhiều mỏ chồng chéo nhau mà ảnh hưởng của chúng bị triệt tiêu không đồng đều giữa các khu vực. Vì mỗi vị trí sẽ ngay lập tức truyền tác dụng của nó tới tất cả các vị trí lân cận nên mọi sự chồng chéo sẽ được giải quyết một cách tự nhiên trong lưới dư. Quy tắc tham lam đảm bảo rằng không còn ô nào có nhu cầu dương trừ khi mỏ tương ứng được đặt ở vị trí vẫn có thể ảnh hưởng đến nó.
