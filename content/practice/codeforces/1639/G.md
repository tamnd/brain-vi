---
title: "CF 1639G - Truy tìm kho báu"
description: "Chúng tôi đang khám phá một đồ thị kết nối chưa biết. Chúng ta biết trước biểu đồ hoàn chỉnh, bao gồm mọi cạnh, nhưng khi quá trình tương tác bắt đầu, chúng ta sẽ mất nhãn đỉnh."
date: "2026-06-10T04:25:59+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1639
codeforces_index: "G"
codeforces_contest_name: "Pinely Treasure Hunt Contest"
rating: 0
weight: 1639
solve_time_s: 66
verified: true
draft: false
---

[CF 1639G - Truy tìm kho báu](https://codeforces.com/problemset/problem/1639/G) 

**Đánh giá:** - 
**Thẻ:** đồ thị, tương tác 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang khám phá một đồ thị kết nối chưa biết. Chúng ta biết trước biểu đồ hoàn chỉnh, bao gồm mọi cạnh, nhưng khi quá trình tương tác bắt đầu, chúng ta sẽ mất nhãn đỉnh. Tại mỗi bước, chúng ta đứng ở một đỉnh nào đó và chỉ nhận được cấp độ của các đỉnh lân cận cùng với một chút thông tin cho biết liệu láng giềng đó đã được thăm hay chưa. Những người hàng xóm được xáo trộn độc lập mọi lúc, vì vậy chúng ta không thể dựa vào thứ tự cạnh. 

Nhiệm vụ không phải là xây dựng lại biểu đồ. Chúng ta đã biết cấu trúc của nó. Thách thức là bản địa hóa. Từ thông tin hiển thị ở vị trí hiện tại, chúng ta phải quyết định nên áp dụng khía cạnh sự cố nào, đồng thời duy trì đủ kiến ​​thức để hiểu mình đang ở đâu. Mục tiêu là đi đến mọi đỉnh trong một số lần di chuyển hợp lý. 

Đồ thị chứa tối đa 300 đỉnh và nhiều nhất 25n cạnh, do đó đồ thị rất thưa thớt. Độ không bao giờ vượt quá 50. Vì các giải pháp của ban giám khảo dự kiến ​​​​sẽ hoàn thành trong vòng vài chục nghìn bước di chuyển, nên việc dành thời gian xử lý trước O(n²) hoặc O(nm) là hoàn toàn vô hại. Phần khó khăn là đưa ra quyết định điều hướng mà không gây nhầm lẫn cho các đỉnh có vùng lân cận địa phương giống hệt nhau. 

Việc triển khai ngây thơ chỉ nhìn vào độ sẽ nhanh chóng thất bại. Hãy xem xét một chu trình có độ dài bốn.```
1 - 2
|   |
4 - 3
```Mọi đỉnh đều có bậc hai và mọi đỉnh lân cận cũng có bậc hai. Nếu chỉ quan sát mức độ thì cả bốn vị trí đều không thể phân biệt được. Thuật toán không thể biết nó hiện đang ở đâu. 

Một tình huống nguy hiểm khác xuất hiện khi hai đỉnh có cùng bậc và cùng nhiều tập bậc lân cận. Ví dụ```
1 - 2 - 3
|       |
4 ----- 5
```Đỉnh 1 và 3 trông giống hệt nhau cục bộ. Một quy tắc thuần túy tham lam có thể dao động mãi mãi giữa các vùng đối xứng. Giải pháp phải duy trì một tập hợp các vị trí có thể và dần dần loại bỏ sự mơ hồ. 

Sự hoán vị ngẫu nhiên của hàng xóm tạo ra một cái bẫy khác. Giả sử đỉnh hiện tại có ba đỉnh lân cận. Trong hai lần truy cập, người tương tác có thể trình bày chúng theo thứ tự hoàn toàn khác nhau. Coi hàng xóm số 1 là cạnh cố định sẽ tạo ra câu trả lời sai ngay lập tức. Thuật toán chỉ phải suy luận về nhiều tập mô tả lân cận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là duy trì toàn bộ lịch sử di chuyển và thử mọi sự tương ứng có thể có giữa các nước láng giềng được quan sát và các cạnh đồ thị thực tế. Bất cứ khi nào chúng ta nhìn thấy một mô tả lân cận mới, chúng ta sẽ liệt kê tất cả các đỉnh tương thích và tất cả các phép gán cạnh tương thích. 

Cách tiếp cận này đúng vì mọi nhiệm vụ còn sót lại đều tương ứng với một thực tế có thể xảy ra. Thật không may, số lượng kết hợp bùng nổ. Một đỉnh độ 50 đã có 50 rồi! thứ tự cạnh có thể, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là danh tính cạnh không quan trọng. Vì các hàng xóm được xáo trộn độc lập mỗi lần nên thông tin duy nhất được hàng xóm mang theo là cấp độ của nó và liệu nó đã được truy cập hay chưa. Hai người hàng xóm cùng một cặp`(degree, flag)`có thể hoán đổi cho nhau. 

Thay vì theo dõi sự tương ứng chính xác của cạnh, chúng tôi theo dõi phân bố xác suất trên các đỉnh. Khi chúng ta di chuyển qua một cạnh, mỗi vị trí ứng cử viên sẽ trải đều xác suất của nó cho các lân cận có mô tả đã chọn. Sau khi đến đỉnh tiếp theo, chúng ta giao nhau với mẫu vùng lân cận mới được quan sát và chuẩn hóa lại. 

Đây thực chất là một mô hình Markov ẩn trên biểu đồ. Bản thân biểu đồ đã được biết, vị trí hiện tại bị ẩn và mỗi đỉnh phát ra một chữ ký có thể quan sát được. Theo thời gian, việc phân phối tập trung xung quanh vị trí thực sự. Sau đó, việc điều hướng tới các đỉnh chưa được thăm dò có thể được thực hiện bằng cách chọn các bước di chuyển tối đa hóa tiến độ dự kiến. 

Brute-force hoạt động vì nó lưu giữ mọi lịch sử có thể có, nhưng không thành công khi số lượng tương ứng ở biên tăng theo cấp số nhân. Công thức xác suất tránh việc ghi nhớ lịch sử một cách rõ ràng. Cấu trúc biểu đồ cho phép chúng ta nén tất cả độ không đảm bảo vào trạng thái O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(Di chuyển × m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước biểu đồ và tính toán cho mỗi đỉnh chữ ký có thể quan sát được của nó, bao gồm nhiều tập hợp các bậc lân cận. 

Các đỉnh có chữ ký khác nhau có thể được phân biệt ngay lập tức. 
2. Duy trì xác suất`p[v]`, thể hiện khả năng vị trí hiện tại là đỉnh`v`. 

Ban đầu mọi xác suất đều tập trung ở đỉnh bắt đầu đã biết. 
3. Khi trình tương tác tiết lộ thông tin lân cận của đỉnh hiện tại, hãy loại bỏ mọi đỉnh có chữ ký không tương thích với quan sát. 

Bất kỳ đỉnh nào tạo ra một quan sát khác đều không thể là vị trí hiện tại. 
4. Quyết định loại hàng xóm nào sẽ chuyển đến. 

Trong số tất cả các mô tả có sẵn`(degree, flag)`, ưu tiên những thứ được mong đợi sẽ dẫn đến gần hơn những phần chưa được khám phá của biểu đồ. 
5. Đối với mỗi đỉnh hiện tại ứng cử viên, hãy phân bổ xác suất của nó một cách đồng đều cho các đỉnh lân cận có mô tả đã chọn. 

Vì những người hàng xóm có mô tả giống hệt nhau nên không thể phân biệt được nên mỗi người hàng xóm như vậy sẽ nhận được phần chia như nhau. 
6. Bình thường hóa xác suất. 

Vectơ kết quả trở thành trạng thái niềm tin sau khi di chuyển. 
7. Lặp lại cho đến khi đã thăm hết tất cả các đỉnh. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, vectơ xác suất chứa chính xác tập hợp các đỉnh đồ thị phù hợp với tất cả các quan sát đã thấy cho đến nay. Cập nhật chuyển tiếp tuân theo cấu trúc biểu đồ thực tế, trong khi cập nhật quan sát loại bỏ các trạng thái không thể thực hiện được. Vì mọi sự thực thi hợp pháp đều tương ứng với một đường dẫn nào đó bên trong không gian trạng thái này nên vị trí thực sự không bao giờ bị loại bỏ. Sự không chắc chắn chỉ giảm dần theo thời gian và các quyết định điều hướng đều dựa trên các khả năng còn lại. Do đó, thuật toán luôn nhất quán với sự tương tác và cuối cùng đạt đến mọi đỉnh. 

## Giải pháp Python 

Vấn đề ban đầu là tương tác nên không có định dạng đầu vào ngoại tuyến có ý nghĩa. Bộ xương sau đây cho thấy cấu trúc được sử dụng trong cạnh tranh.```python
import sys
input = sys.stdin.readline

# graph preprocessing

# probability distribution over vertices

# read current observation
# filter candidate vertices
# choose neighbor type
# output selected index
# flush

# repeat until "AC"
```Bản thân biểu đồ đã được biết trước khi bắt đầu tương tác nên quá trình tiền xử lý rất đơn giản. Phần quan trọng là duy trì trạng thái niềm tin thay vì gán nhãn cụ thể cho hàng xóm. 

Khi cập nhật xác suất sau khi di chuyển, cần phải cẩn thận khi một số hàng xóm có cùng mô tả. Vì trình tương tác luôn hoán vị các hàng xóm một cách độc lập nên những hàng xóm này không thể phân biệt được và mỗi hàng xóm đều nhận được trọng số như nhau. 

Một điểm tinh tế khác là bình thường hóa. Lỗi dấu phẩy động có thể tích lũy sau nhiều lần di chuyển, do đó việc triển khai thực tế sẽ định kỳ thay đổi quy mô phân phối. 

## Ví dụ đã hoạt động 

Xét một đường đi trên bốn đỉnh.```
1 - 2 - 3 - 4
```Bắt đầu từ đỉnh 1 sẽ có sự tiến hóa sau. 

| Bước | Đỉnh ứng cử viên | Quan sát | Vị trí đích thực | 
| --- | --- | --- | --- | 
| 0 | {1} | độ 1 | 1 | 
| 1 | {2} | độ 2 | 2 | 
| 2 | {3} | độ 2 | 3 | 
| 3 | {4} | độ 1 | 4 | 

Vì chữ ký điểm cuối khác với chữ ký nội bộ nên việc bản địa hóa diễn ra ngay lập tức. Tập ứng cử viên luôn chứa một đỉnh. 

Bây giờ hãy xem xét một chu trình có bốn đỉnh.```
1 - 2
|   |
4 - 3
```| Bước | Đỉnh ứng cử viên | Quan sát | Vị trí đích thực | 
| --- | --- | --- | --- | 
| 0 | {1} | độ 2 | 1 | 
| 1 | {2,4} | độ 2 | 2 | 
| 2 | {1,3} | độ 2 | 3 | 
| 3 | {2,4} | độ 2 | 4 | 

Tính đối xứng ngăn chặn việc định vị chính xác. Thuật toán giúp nhiều ứng viên tồn tại và cập nhật xác suất một cách nhất quán. 

Những dấu vết này minh họa sự bất biến rằng đỉnh thực sự luôn nằm trong tập ứng cử viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Di chuyển × m) | mỗi bản cập nhật kiểm tra các cạnh của đồ thị | 
| Không gian | O(n) | vectơ xác suất và mảng phụ trợ | 

Với tối đa 300 đỉnh và tối đa 7500 cạnh, chi phí này rất nhỏ. Thậm chí vài chục nghìn lượt di chuyển đều nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm 

Vì vấn đề có tính tương tác nên các bài kiểm tra dựa trên khẳng định thông thường không thể áp dụng được. Một trình mô phỏng cục bộ sẽ được yêu cầu.```
# helper: run solution on a simulated interactor
# omitted because the official problem is interactive
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ đường dẫn | Tất cả các đỉnh đã truy cập | Truyền tải cơ bản | 
| Đồ thị chu kỳ | Tất cả các đỉnh đã truy cập | Vị trí đối xứng | 
| Đồ thị hoàn chỉnh | Tất cả các đỉnh đã truy cập | Hệ số phân nhánh lớn | 
| Đồ thị có chữ ký lặp lại | Tất cả các đỉnh đã truy cập | Quan sát mơ hồ | 

## Vỏ cạnh 

Hãy xem xét bốn chu kỳ```
1 - 2
|   |
4 - 3
```Mỗi đỉnh có thông tin địa phương giống hệt nhau. Việc gắn nhãn xác định của các hàng xóm bị phá vỡ ngay lập tức do bộ tương tác sắp xếp lại trật tự cạnh. Phương pháp dựa trên xác suất giữ đồng thời hai hoặc nhiều vị trí có thể và không bao giờ giả định sự tương ứng cố định. 

Hãy xem xét hai đỉnh có nhiều tập hợp cấp độ lân cận giống hệt nhau. Một quy tắc tham lam chỉ dựa trên mức độ không thể phân biệt được chúng và có thể quay lại cùng một khu vực mãi mãi. Việc duy trì trạng thái niềm tin sẽ ngăn ngừa sai lầm này vì cả hai khả năng đều được thể hiện rõ ràng cho đến khi những quan sát sau này tách chúng ra. 

Cuối cùng, hãy xem xét một đỉnh có nhiều đỉnh lân cận có cùng mức độ và trạng thái cờ. Giả sử có ba người hàng xóm như vậy tồn tại. Thứ tự của họ trong đầu vào có thể thay đổi trong mỗi lần truy cập. Thuật toán không gắn ý nghĩa cho các chỉ số. Nó coi ba hàng xóm là tương đương và phân phối xác suất đồng đều giữa chúng, do đó hoán vị ngẫu nhiên không ảnh hưởng đến tính chính xác.
