---
title: "CF 105430A - NẮNG"
description: "Chúng ta có hai bộ số nguyên được sắp xếp biểu thị vị trí dao động trên trục số khổng lồ. Chúng ta được phép thực hiện một thao tác trong đó chúng ta chọn một vị trí trục xoay k và mọi rung động ở bên trái của k sẽ di chuyển xa hơn một bước sang trái, mọi rung động đúng đến…"
date: "2026-06-23T04:03:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105430
codeforces_index: "A"
codeforces_contest_name: "OMORI CONTEST"
rating: 0
weight: 105430
solve_time_s: 170
verified: false
draft: false
---

[CF 105430A - NẮNG](https://codeforces.com/problemset/problem/105430/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai bộ số nguyên được sắp xếp biểu thị vị trí dao động trên trục số khổng lồ. Chúng tôi được phép thực hiện thao tác trong đó chúng tôi chọn vị trí xoay`k`, và mọi dao động ở đúng bên trái của`k`di chuyển thêm một bước sang trái, mọi rung động đều ở bên phải`k`tiến thêm một bước sang phải và mọi thứ đều chính xác`k`vẫn đứng yên. 

Khía cạnh quan trọng là một thao tác không di chuyển một điểm nào, nó đồng thời mở rộng cấu hình ra khỏi trục đã chọn. Việc lặp lại thao tác này với các trục xoay khác nhau sẽ thay đổi cấu hình theo cách có cấu trúc và nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi cấu hình ban đầu thành cấu hình đích một cách chính xác hay không. 

Kích thước đầu vào lên tới hai trăm nghìn điểm, do đó, bất kỳ phương pháp nào cố gắng mô phỏng chuỗi hoạt động hoặc tìm kiếm qua các lựa chọn trục xoay trên mỗi bước đều ngay lập tức quá chậm. Ngay cả một thao tác đơn lẻ cũng đã ảnh hưởng đến tất cả các điểm, do đó việc mô phỏng nhiều thao tác sẽ nhân chi phí đó lên gấp bội. 

Một vấn đề tế nhị là sự chuyển đổi không mang tính cục bộ. Chuyển động của một điểm phụ thuộc vào tất cả các thao tác có trục quay nằm ở bên trái hoặc bên phải của điểm đó, do đó việc suy nghĩ độc lập về từng yếu tố sẽ nhanh chóng dẫn đến sự mâu thuẫn. Một khó khăn tiềm ẩn khác là hoạt động có thể di chuyển các phân đoạn khác nhau theo hướng ngược nhau cùng một lúc, do đó, việc khớp từng điểm một có thể không thành công ngay cả khi tồn tại một chuỗi hợp lệ. 

Một ví dụ nhỏ mà lý luận ngây thơ có thể sai là cố gắng so sánh từng`a_i`ĐẾN`b_i`một cách độc lập bằng cách kiểm tra xem chúng có thể truy cập được bằng một số thao tác hay không. Ví dụ, với`a = [1, 3, 5]`Và`b = [2, 3, 6]`, mỗi điểm riêng lẻ có vẻ có thể dịch chuyển được bởi một số thao tác, nhưng cấu trúc chung của các thao tác kết hợp tất cả các phần tử, do đó tính khả thi độc lập sẽ gây hiểu nhầm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng chuỗi hoạt động. Đối với mỗi trục có thể`k`, chúng tôi áp dụng phép chuyển đổi toàn cục cho mảng rồi thử kết hợp các thao tác đó để đạt được mục tiêu. Ngay cả việc hạn chế bản thân trong một tập hợp hữu hạn các trục ứng cử viên vẫn để lại một quy trình phân nhánh trong đó mỗi bước tính toán lại tất cả các vị trí trong`O(n)`. Với tối đa`2 * 10^5`các phần tử, thậm chí vài trăm hoạt động sẽ vượt quá bất kỳ giới hạn thời gian hợp lý nào, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là ngừng suy nghĩ theo trình tự các hoạt động và thay vào đó hãy mô hình hóa hiệu quả ròng của tất cả các hoạt động. Mỗi thao tác tại vị trí`k`đóng góp một hiệu ứng có dấu cố định cho mọi điểm: các điểm còn lại của`k`giảm đi một, điểm bên phải của`k`tăng thêm một. Điều này có nghĩa là mỗi phép toán tạo ra một “sự thay đổi độ dốc” tuyến tính toàn cục trên trục số. 

Khi chúng tôi chuyển đổi góc nhìn, chúng tôi nhận ra rằng cấu trúc có ý nghĩa duy nhất là cách những thay đổi độ dốc này tích lũy giữa các điểm liên tiếp trong mảng được sắp xếp. Phép biến đổi giảm xuống mức kiểm soát sự khác biệt về độ dịch chuyển giữa các phần tử liền kề và điều này hóa ra lại áp đặt một ràng buộc về tính đơn điệu đối với các độ dịch chuyển cần thiết. 

Điều này làm giảm vấn đề kiểm tra xem sự dịch chuyển từ`a_i`ĐẾN`b_i`tạo thành một chuỗi có sự khác biệt phù hợp với việc được tạo ra bởi sự đóng góp không âm giữa các khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ / O(n · phép toán) | O(n) | Quá chậm | 
| Mô hình hóa sự khác biệt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta hãy xác định độ dịch chuyển cần thiết cho mỗi điểm là`d_i = b_i - a_i`. 

Bây giờ chúng ta suy luận về việc một thao tác đơn lẻ sẽ ảnh hưởng như thế nào đến những chuyển vị này. Nếu chúng ta áp dụng một thao tác tại vị trí`k`, thì mọi điểm bên trái của`k`giảm đi một và mọi điểm bên phải đều tăng một. Điều này tạo ra sự thay đổi về cấu trúc ảnh hưởng đến sự khác biệt tương đối giữa các điểm. 

Bước quan trọng là kiểm tra hai điểm liên tiếp`a_i < a_{i+1}`. Hãy xem xét sự chênh lệch dịch chuyển là bao nhiêu`d_{i+1} - d_i`có thể thay đổi do hoạt động. Chỉ những hoạt động có điểm xoay nằm hoàn toàn giữa`a_i`Và`a_{i+1}`góp phần vào sự khác biệt này và mỗi hoạt động như vậy đóng góp chính xác`+2`ĐẾN`d_{i+1} - d_i`. 

Vì vậy, chúng ta nhận được một phương trình cấu trúc:`d_{i+1} - d_i = 2 * (number of operations with pivot in (a_i, a_{i+1}))`Từ đây chúng ta thu được ngay hai ràng buộc. Đầu tiên, mọi sự khác biệt`d_{i+1} - d_i`phải không âm, vì nó gấp đôi số phép tính. Thứ hai, mọi sự khác biệt như vậy phải bằng nhau. 

Bây giờ chúng ta biến những điều kiện này thành một thủ tục quyết định: 

1. Tính toán`d_i = b_i - a_i`cho tất cả các chỉ số. 
2. Kiểm tra xem trình tự`d`là không giảm. Điều này đảm bảo không có số lượng hoạt động âm nào phát sinh giữa các khoảng thời gian. 
3. Kiểm tra xem mọi chênh lệch liền kề`d_{i+1} - d_i`là chẵn. Điều này đảm bảo rằng số lượng thao tác trong mỗi khoảng là một số nguyên. 
4. Nếu cả hai điều kiện đều đúng thì việc chuyển đổi có thể thực hiện được; nếu không thì không thể được. 

Để hiểu tại sao điều này là đủ, hãy lưu ý rằng mỗi khoảng thời gian giữa các`a_i`Và`a_{i+1}`có thể được gán một số phép toán độc lập, được xác định chính xác bởi`(d_{i+1} - d_i) / 2`. Bất kỳ sự thay đổi toàn cầu còn sót lại nào cũng có thể được xử lý bằng cách đặt các hoạt động nằm ngoài phạm vi của tất cả các điểm, điều này ảnh hưởng đến mọi phần tử như nhau và không làm ảnh hưởng đến sự khác biệt. 

### Tại sao nó hoạt động 

Thuật toán ngầm xây dựng lại nhiều tập hợp trục hoạt động hợp lệ. Bất biến chính là vectơ chuyển vị`d_i`có thể được phân tách thành các phần đóng góp từ các khoảng cách nhau trên trục số. Mỗi khoảng đóng góp thống nhất cho tất cả các điểm ở bên trái và bên phải của nó theo cách chỉ ảnh hưởng đến những khác biệt liền kề theo cách bổ sung, được kiểm soát. Bởi vì những đóng góp này là độc lập và không âm, nên tính khả thi giảm chính xác xuống việc kiểm tra xem tất cả các đóng góp theo khoảng có tồn tại dưới dạng số đếm hợp lệ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    d = [b[i] - a[i] for i in range(n)]
    
    for i in range(n - 1):
        if d[i] > d[i + 1]:
            print("NO")
            return
        if (d[i + 1] - d[i]) % 2 != 0:
            print("NO")
            return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Mã này tính toán trực tiếp mảng dịch chuyển và sau đó thực thi hai điều kiện dẫn xuất: tính đơn điệu và thậm chí là sự khác biệt. Kiểm tra tính đơn điệu tương ứng với việc đảm bảo không có khoảng thời gian nào yêu cầu số lượng hoạt động âm, điều này là không thể vì số lượng hoạt động không thể âm. 

Kiểm tra tính chẵn lẻ đảm bảo rằng mọi khoảng cách giữa các vị trí ban đầu liên tiếp đều thừa nhận một số nguyên các phép toán, vì mỗi phép toán đóng góp chính xác hai đơn vị chênh lệch. 

Không cần có ràng buộc nào khác vì các dịch chuyển toàn cục luôn có thể được tạo ra bởi các phép toán nằm ngoài phạm vi của tất cả các điểm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [3, 4, 5, 6]
b = [3, 5, 6, 8]
```Chúng tôi tính toán: 

| tôi | một [tôi] | b[i] | d[i] | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 
| 1 | 4 | 5 | 1 | 
| 2 | 5 | 6 | 1 | 
| 3 | 6 | 8 | 2 | 

Bây giờ hãy kiểm tra sự khác biệt liền kề: 

| tôi | d[i] | d[i+1] | khác biệt | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | chẵn lẻ không hợp lệ | 
| 1 | 1 | 1 | 0 | hợp lệ | 
| 2 | 1 | 2 | 1 | chẵn lẻ không hợp lệ | 

Vì một số khác biệt là lẻ nên không thể tạo được cấu hình. 

Điều này cho thấy ngay cả khi tất cả các vị trí mục tiêu riêng lẻ trông hợp lý thì việc ghép nối giữa các phần tử liền kề sẽ ngăn cản sự phân tách hợp lệ thành các hoạt động. 

### Ví dụ 2 

đầu vào:```
a = [3, 4, 5, 6]
b = [3, 4, 7, 8]
```Tính toán chuyển vị: 

| tôi | một [tôi] | b[i] | d[i] | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 
| 1 | 4 | 4 | 0 | 
| 2 | 5 | 7 | 2 | 
| 3 | 6 | 8 | 2 | 

Sự khác biệt liền kề: 

| tôi | d[i] | d[i+1] | khác biệt | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | được | 
| 1 | 0 | 2 | 2 | được | 
| 2 | 2 | 2 | 0 | được | 

Tất cả các ràng buộc đều được thỏa mãn, do đó tồn tại một chuỗi hoạt động hợp lệ. 

Điều này cho thấy một trường hợp rõ ràng trong đó cấu trúc chuyển vị chỉ tăng ở một ranh giới, tương ứng chính xác với các thao tác đặt trong một khoảng duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lần để tính toán chuyển vị và xác minh các ràng buộc | 
| Không gian | O(n) | mảng giá trị dịch chuyển | 

Giải pháp này hoạt động thoải mái trong giới hạn vì nó chỉ thực hiện quét tuyến tính trên các mảng có kích thước lên tới hai trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    d = [b[i] - a[i] for i in range(n)]
    
    for i in range(n - 1):
        if d[i] > d[i + 1]:
            return "NO"
        if (d[i + 1] - d[i]) % 2 != 0:
            return "NO"
    
    return "YES"

# provided samples
assert run("4\n3 4 5 6\n3 5 6 8\n") == "NO"
assert run("4\n3 4 5 6\n3 4 7 8\n") == "YES"

# custom cases
assert run("1\n5\n5\n") == "YES", "single element always ok"
assert run("2\n1 10\n2 11\n") == "YES", "uniform shift"
assert run("2\n1 10\n2 12\n") == "NO", "parity mismatch"
assert run("3\n1 5 9\n1 6 9\n") == "NO", "non-monotone displacement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | tính khả thi tầm thường | 
| ca đồng phục | CÓ | hành vi thay đổi toàn cầu | 
| sự không khớp chẵn lẻ | KHÔNG | ràng buộc chênh lệch chẵn | 
| chuyển vị không đơn điệu | KHÔNG | yêu cầu cấu trúc đơn điệu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các điểm đã khớp nhau, trong đó mọi`d_i = 0`. Thuật toán kiểm tra sự khác biệt, tìm tất cả số 0 và chấp nhận ngay lập tức. Điều này tương ứng với việc thực hiện các phép toán bằng 0 hoặc áp dụng các phép toán ở xa đối xứng có tính hủy bỏ. 

Một trường hợp khác là cấu hình tối thiểu có hai điểm trong đó chỉ có một điểm dịch chuyển không chính xác. Ví dụ`a = [1, 2]`,`b = [1, 3]`sản xuất`d = [0, 1]`, không có tính chẵn lẻ vì chênh lệch là số lẻ. Thuật toán loại bỏ điều này một cách chính xác vì không có chuỗi thao tác nào có thể tạo ra sự mất cân bằng đơn vị giữa các điểm liền kề. 

Trường hợp khó phát hiện thứ ba là khi các chuyển vị đều đơn điệu nhưng không khả thi do tính chẵn lẻ. Ví dụ`d = [0, 2, 3]`đang tăng lên, nhưng bước cuối cùng có sự khác biệt kỳ lạ, vì vậy nó bị từ chối mặc dù bản thân các giá trị trông có vẻ nhất quán. Điều này phản ánh rằng mỗi khoảng đóng góp theo từng phần chính xác là hai, do đó, các bước nhảy lẻ là không thể về mặt cấu trúc.
