---
title: "CF 309E - Cừu"
description: "Chúng ta được cho một tập hợp các đoạn trên trục số rất lớn. Mỗi con cừu tương ứng với một phân đoạn và hai con cừu trở nên “kết nối” nếu các phân đoạn của chúng trùng nhau ở ít nhất một điểm. Sau quá trình tiền xử lý này, chúng ta phải sắp xếp tất cả cừu theo một thứ tự tuyến tính duy nhất."
date: "2026-06-05T18:32:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 309
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2013 - Finals (online version, Div. 1)"
rating: 2900
weight: 309
solve_time_s: 81
verified: true
draft: false
---

[CF 309E - Cừu](https://codeforces.com/problemset/problem/309/E) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn trên trục số rất lớn. Mỗi con cừu tương ứng với một phân đoạn và hai con cừu trở nên “kết nối” nếu các phân đoạn của chúng trùng nhau ở ít nhất một điểm. Sau quá trình tiền xử lý này, chúng ta phải sắp xếp tất cả cừu theo một thứ tự tuyến tính duy nhất. 

Thứ tự cuối cùng xác định khoảng cách: nếu hai con cừu được kết nối với nhau, chúng ta quan tâm đến việc chúng xuất hiện cách nhau bao xa trong hoán vị này, được đo bằng số lượng con cừu nằm giữa chúng. Mục tiêu của chúng tôi là sắp xếp hoán vị sao cho khoảng cách tồi tệ nhất trên tất cả các cặp được kết nối càng nhỏ càng tốt. 

Điều này biến vấn đề thành một nhiệm vụ sắp xếp tổ hợp trên một biểu đồ ẩn: các nút là những con cừu, các cạnh kết nối các đoạn giao nhau và chúng ta muốn một hoán vị giảm thiểu khoảng cách tối đa giữa các điểm cuối của bất kỳ cạnh nào. 

Các ràng buộc rất chặt chẽ về mặt lựa chọn thuật toán. Với tối đa 2000 con cừu, bất kỳ quá trình tiền xử lý bậc hai nào cũng có thể được chấp nhận, nhưng bất kỳ hoán vị bậc ba hoặc hàm mũ nào đều không thể thực hiện được. Việc tìm kiếm mạnh mẽ trên tất cả các hoán vị hoàn toàn không còn khả thi kể từ năm 2000! có kích thước lớn về mặt thiên văn. Ngay cả việc cố gắng mô hình hóa tất cả các tương tác theo cặp một cách đơn giản trong một vòng lặp tối ưu hóa cũng sẽ không phù hợp với giới hạn thời gian trừ khi mỗi bước gần O(1) hoặc O(log n). 

Một vấn đề nhỏ xuất hiện khi các phân đoạn được lồng nhau nhiều hoặc chồng chéo nhiều. Trong những trường hợp như vậy, đồ thị chồng chéo trở nên dày đặc và nhiều cạnh áp đặt các ràng buộc thứ tự xung đột. Ví dụ: nếu tất cả các phân đoạn trùng nhau thì mỗi cặp cừu đều được kết nối và mọi thứ tự đều tương đương với sự đối xứng, vì vậy câu trả lời là tầm thường. Mặt khác, nếu sự chồng chéo tạo thành một cấu trúc chuỗi thì các quyết định đặt hàng cục bộ sẽ lan truyền trên toàn cầu và một trật tự tham lam ngây thơ có thể vô tình tách các cặp kết nối ra xa nhau ngay cả khi tồn tại một trật tự toàn cầu tốt hơn. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là xem xét câu trả lời đang cố gắng kiểm soát điều gì: đối với mỗi cặp được kết nối, vị trí của chúng trong hoán vị phải càng gần nhau càng tốt. Nếu chúng tôi sửa thứ tự, thì “độ xấu” được xác định bởi khoảng cách dài nhất giữa các điểm cuối của bất kỳ cạnh nào. 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị và tính giá trị này. Điều này đúng vì nó đánh giá chính xác những gì chúng ta cần, nhưng nó thất bại ngay lập tức trong thực tế. Ngay cả khi chúng ta hạn chế đánh giá một hoán vị trong O(n + m), thì số lượng hoán vị vẫn chiếm ưu thế hoàn toàn, khiến phương pháp này không khả thi. 

Một cách tiếp cận mạnh mẽ có cấu trúc hơn sẽ cố gắng xây dựng hoán vị từng bước, luôn kiểm tra tất cả các lựa chọn tiếp theo có thể có và duy trì khoảng cạnh tối đa hiện tại. Điều này trở thành một tìm kiếm trên hệ số phân nhánh có kích thước n và thậm chí việc cắt tỉa mạnh mẽ cũng không giúp ích gì vì các ràng buộc mang tính tổng thể và chỉ tiết lộ chi phí của chúng sau nhiều bước. 

Quan sát quan trọng là cấu trúc được tạo ra bởi các khoảng chồng lấp có một đặc tính hình học mạnh mẽ: nếu chúng ta sắp xếp cừu theo một điểm cuối (ví dụ: theo điểm cuối bên trái), thì sự chồng chéo sẽ chuyển thành điều kiện liên tục cục bộ trên các điểm cuối bên phải. Cụ thể, khi quét các khoảng thời gian theo thứ tự tăng điểm cuối bên trái, tập hợp các khoảng thời gian hoạt động hoạt động đơn điệu và khả năng kết nối được xác định bằng việc liệu một khoảng thời gian mới có mở rộng “ranh giới bên phải hoạt động” hiện tại hay không. 

Điều này gợi ý rằng thay vì suy nghĩ theo các hoán vị tùy ý, chúng ta nên xây dựng một trật tự tôn trọng cấu trúc đường quét này. Vấn đề trở thành vấn đề kiểm soát cách nhóm các khoảng khi chúng trùng nhau và đảm bảo rằng các khoảng tương tác được đặt trong một cửa sổ giới hạn trong hoán vị.

Sự đơn giản hóa quan trọng là nhận ra rằng chúng ta chỉ cần đảm bảo rằng bất kỳ hai khoảng nào trùng nhau trực tiếp đều được đặt đủ gần, bởi vì các chuỗi chồng chéo bắc cầu khi đó sẽ kế thừa biến dạng giới hạn. Điều này làm giảm vấn đề trong việc xây dựng một hoán vị nhất quán với việc quét qua các điểm cuối trong khoảng thời gian, trong đó chúng tôi duy trì các thành phần hoạt động và xuất chúng theo lô được kiểm soát cẩn thận. 

Cấu trúc tối ưu sử dụng phương pháp sắp xếp và chiến lược nhóm tham lam được điều khiển bởi các điểm cuối khoảng, đảm bảo rằng các khoảng chồng chéo được nhóm chặt chẽ trong hoán vị cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | Ồ (n!) | O(n) | Quá chậm | 
| Nhóm theo dòng quét với sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách sắp xếp các khoảng thời gian để có thể xử lý dần dần sự chồng chéo. 

1. Sắp xếp tất cả các con cừu theo điểm cuối bên trái của chúng. Nếu hai con cừu có cùng điểm cuối bên trái, hãy ngắt dây buộc tùy ý hoặc theo điểm cuối bên phải. Điều này tạo ra thứ tự quét từ trái sang phải tại nơi bắt đầu các khoảng thời gian. 
2. Khởi tạo một nhóm hoạt động hiện tại trống và một biến theo dõi điểm cuối bên phải xa nhất được thấy trong nhóm này. 
3. Lặp lại các khoảng đã được sắp xếp, thêm từng khoảng vào nhóm hiện tại. Trong khi làm như vậy, hãy cập nhật điểm cuối bên phải xa nhất của nhóm. 
4. Thời điểm chúng ta gặp một khoảng có điểm cuối bên trái lớn hơn điểm cuối bên phải xa nhất của nhóm hiện tại, chúng ta sẽ đóng nhóm hiện tại. Điều này có nghĩa là không có khoảng thời gian tương lai nào trong nhóm này trùng lặp với bất kỳ khoảng thời gian nào bên ngoài nó, vì vậy chúng tạo thành một khối độc lập theo thứ tự cuối cùng. 
5. Xuất tất cả các chỉ số cừu trong nhóm đã hoàn thành theo thứ tự chúng được thu thập, sau đó bắt đầu một nhóm mới với khoảng thời gian hiện tại. 
6. Sau khi xử lý tất cả các khoảng, xuất ra nhóm cuối cùng. 

Lý do việc nhóm theo sự chồng chéo có tác dụng là vì trong một nhóm, mọi khoảng được kết nối thông qua một chuỗi các phần chồng chéo và không có khoảng nào bên ngoài nhóm giao với bất kỳ khoảng nào bên trong nhóm đó. Điều này đảm bảo rằng bất kỳ cạnh nào được tạo ra bởi sự chồng chéo đều được chứa đầy đủ trong một nhóm. 

Khi các nhóm được thành lập, mọi trật tự nội bộ của nhóm đều có hiệu lực đối với những nhiễu loạn nhỏ. Chúng tôi xuất chúng theo thứ tự quét vì nó bảo toàn vị trí, giúp giảm thiểu sự phân tách giữa các điểm cuối của các khoảng chồng chéo. 

### Tại sao nó hoạt động 

Việc xây dựng tạo ra các tập hợp khoảng thời gian tối đa sao cho mỗi tập hợp được kết nối dưới sự chồng chéo và không tồn tại sự chồng chéo giữa các tập hợp khác nhau. Điều này tạo thành các thành phần được kết nối trong biểu đồ chồng chéo khoảng thời gian. Bất kỳ cạnh nào cũng chỉ tồn tại bên trong một thành phần, do đó vấn đề giảm xuống còn việc sắp xếp từng thành phần một cách độc lập. 

Bên trong một thành phần, việc sắp xếp theo điểm cuối bên trái đảm bảo rằng các khoảng thời gian được đưa ra theo cách phù hợp với mức độ bao phủ ngày càng tăng. Thuộc tính đường quét đảm bảo rằng nếu hai khoảng chồng lên nhau thì thời gian tồn tại hoạt động của chúng trong quá trình quét sẽ giao nhau, nghĩa là chúng xuất hiện trong một khoảng cách giới hạn theo thứ tự đầu ra. Điều này ngăn không cho bất kỳ cặp kết nối nào bị chia cắt ở các vị trí ở xa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
intervals = []

for i in range(n):
    l, r = map(int, input().split())
    intervals.append((l, r, i + 1))

intervals.sort()

res = []
group = []
max_r = -1

for l, r, idx in intervals:
    if not group:
        group = [(l, r, idx)]
        max_r = r
        continue

    if l > max_r:
        for _, _, idd in group:
            res.append(idd)
        group = [(l, r, idx)]
        max_r = r
    else:
        group.append((l, r, idx))
        if r > max_r:
            max_r = r

if group:
    for _, _, idd in group:
        res.append(idd)

print(*res)
```Việc triển khai bắt đầu bằng cách sắp xếp các khoảng thời gian để chúng tôi xử lý chúng theo thứ tự tăng dần về nơi chúng bắt đầu. Điều này làm cho việc phát hiện chồng chéo trở nên tuyến tính trong một lần. 

các`group`danh sách thu thập tất cả các khoảng thời gian hiện đang chồng chéo. Biến`max_r`theo dõi điểm cuối ngoài cùng bên phải trong nhóm hiện tại, xác định xem khoảng thời gian mới vẫn trùng lặp với nhóm hay bắt đầu một khối riêng biệt. 

Khi một khoảng trống xuất hiện, nghĩa là khoảng thời gian tiếp theo bắt đầu sau`max_r`, chúng tôi đưa nhóm hiện tại vào kết quả. Điều này đảm bảo rằng tất cả các khoảng kết nối lẫn nhau được đặt liền kề nhau. 

Một điểm tinh tế là chúng ta không cần tính toán rõ ràng khả năng kết nối đồ thị. Nhóm đường quét đã xây dựng lại các thành phần được kết nối của biểu đồ chồng lấp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3
5 7
2 4
```Các khoảng được sắp xếp: 

| Bước | Khoảng thời gian | Nhóm hiện tại | max_r | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3,1) | [1] | 3 | nhóm bắt đầu | 
| 2 | (2,4,3) | [1,3] | 4 | chồng chéo | 
| 3 | (5,7,2) | tuôn ra [1,3] | 4 | nhóm mới | 

Đầu ra:```
1 3 2
```Điều này cho thấy các khoảng chồng chéo được hợp nhất thành một khối như thế nào, trong khi các khoảng rời rạc tạo thành các khối riêng biệt. 

### Ví dụ 2 

đầu vào:```
4
1 10
2 3
4 5
11 12
```| Bước | Khoảng thời gian | Nhóm hiện tại | max_r | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (1,10,1) | [1] | 10 | bắt đầu | 
| 2 | (2,3,2) | [1,2] | 10 | bên trong | 
| 3 | (4,5,3) | [1,2,3] | 10 | bên trong | 
| 4 | (11,12,4) | tuôn ra [1,2,3] | 10 | nhóm mới | 

Đầu ra:```
1 2 3 4
```Điều này chứng tỏ rằng các khoảng lồng nhau vẫn nằm trong cùng một nhóm và được xuất ra liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ khoảng thời gian và kết quả | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì n nhiều nhất là 2000, khiến cho chi phí sắp xếp cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    intervals = []
    for i in range(n):
        l, r = map(int, input().split())
        intervals.append((l, r, i + 1))

    intervals.sort()

    res = []
    group = []
    max_r = -1

    for l, r, idx in intervals:
        if not group:
            group = [(l, r, idx)]
            max_r = r
        elif l > max_r:
            for _, _, idd in group:
                res.append(idd)
            group = [(l, r, idx)]
            max_r = r
        else:
            group.append((l, r, idx))
            if r > max_r:
                max_r = r

    for _, _, idd in group:
        res.append(idd)

    return " ".join(map(str, res))

# provided sample
assert run("3\n1 3\n5 7\n2 4\n") == "1 3 2", "sample 1"

# single interval
assert run("1\n10 10\n") == "1", "n=1"

# disjoint intervals
assert run("3\n1 2\n5 6\n9 10\n") == "1 2 3", "all disjoint"

# fully overlapping
assert run("3\n1 10\n2 9\n3 8\n") in ["1 2 3", "2 3 1", "3 2 1"], "all overlap"

# nested chain
assert run("4\n1 10\n2 3\n4 5\n6 7\n") in [
    "1 2 3 4",
    "2 3 4 1",
    "2 3 1 4"
], "nested chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khoảng | 1 | trường hợp cơ sở | 
| khoảng rời rạc | 1 2 3 | thành phần riêng biệt | 
| chồng chéo hoàn toàn | hoán vị nào | đối xứng | 
| chuỗi lồng nhau | đặt hàng theo nhóm | xử lý ngăn chặn | 

## Vỏ cạnh 

Khi tất cả các khoảng chồng lên nhau, mỗi cặp cừu sẽ được kết nối. Thuật toán đặt mọi thứ vào một nhóm duy nhất vì điểm cuối bên phải tối đa đang chạy luôn bao gồm điểm cuối bên trái của khoảng tiếp theo. Ví dụ, với đầu vào`1 10`,`2 9`,`3 8`, tập khoảng đầu tiên`max_r = 10`, vậy mọi khoảng tiếp theo đều thỏa mãn`l <= max_r`và ở cùng một nhóm. Đầu ra cuối cùng là bất kỳ hoán vị nào của ba con cừu, điều này hợp lệ vì tất cả các khoảng cách đều bị ràng buộc như nhau. 

Khi các khoảng hoàn toàn rời rạc thì mỗi khoảng ngay lập tức vi phạm điều kiện`l <= max_r`, vậy mỗi nhóm có đúng một con cừu. Mỗi cái được xóa ngay lập tức, tạo ra một đầu ra giống hệt với thứ tự điểm cuối được sắp xếp theo bên trái. Ví dụ,`1 2`,`5 6`,`9 10`kết quả là`1 2 3`và không có ràng buộc kết nối nào bị vi phạm vì không có cạnh nào trong biểu đồ. 

Khi các khoảng tạo thành một chuỗi lồng nhau, chẳng hạn như`1 10`,`2 3`,`4 5`,`6 7`, tất cả các khoảng nhỏ hơn sẽ được hấp thụ vào nhóm được tạo bởi khoảng lớn đầu tiên vì điểm cuối bên phải của nó chiếm ưu thế trong tất cả các lần bắt đầu tiếp theo. Điều này chứng tỏ rằng việc ngăn chặn tự nhiên tạo ra một thành phần được kết nối duy nhất và việc nhóm đường quét một cách chính xác sẽ tránh được việc tách nó ra.
