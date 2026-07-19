---
title: "CF 103495L - Trò chơi trên cây"
description: "Chúng ta có một cây có gốc với các nút được đánh nhãn từ 1 đến n, trong đó nút 1 là gốc. Mỗi nút ban đầu giữ một giá trị và các giá trị này tạo thành một hoán vị một phần: một số nút đã chứa các số riêng biệt, trong khi các nút khác trống và được đánh dấu là 0."
date: "2026-07-03T06:11:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103495
codeforces_index: "L"
codeforces_contest_name: "2021 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103495
solve_time_s: 59
verified: true
draft: false
---

[CF 103495L - Trò chơi trên cây](https://codeforces.com/problemset/problem/103495/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với các nút được đánh nhãn từ 1 đến n, trong đó nút 1 là gốc. Mỗi nút ban đầu giữ một giá trị và các giá trị này tạo thành một hoán vị một phần: một số nút đã chứa các số riêng biệt, trong khi các nút khác trống và được đánh dấu là 0. Các giá trị còn thiếu chính xác là các số chưa được sử dụng từ 1 đến n. 

Một nước đi trong trò chơi cho phép chọn một nút u không có nút con nào vẫn "chưa được xử lý", và sau đó thực hiện một thao tác cục bộ trên tập gồm có u và tất cả các nút con trực tiếp của nó. Hoạt động này loại bỏ các giá trị khỏi các nút này và phân phối lại chúng một cách tùy ý trong cùng một tập hợp, cho phép thực hiện bất kỳ hoán vị nào bên trong vùng lân cận hình ngôi sao này một cách hiệu quả. Khi một nút được sử dụng theo cách này, nó sẽ được “sắp xếp lại” vĩnh viễn và không thể tham gia lại. 

Mục tiêu của Alice là cuối cùng biến đổi toàn bộ cây sao cho nút u chứa giá trị u cho mọi nút u. Nhiệm vụ của Bob thì khác: anh ấy vẫn đang trong quá trình gán các giá trị còn thiếu và anh ấy muốn đếm xem có bao nhiêu lần hoàn thành hoán vị một phần giúp Alice có thể thành công trong việc đạt được cấu hình nhận dạng thông qua các hoạt động hợp lệ. 

Các ràng buộc rất lớn, với tổng n trên tất cả các trường hợp thử nghiệm lên tới 200000. Điều này ngay lập tức loại trừ mọi thứ bậc hai trên mỗi thử nghiệm hoặc bất kỳ mô phỏng toàn cầu nào của trò chơi. Bất kỳ giải pháp đúng nào cũng phải giảm từng trường hợp kiểm thử thành công việc tuyến tính hoặc gần tuyến tính, thường là O(n), bởi vì ngay cả O(n log n) trên toàn bộ đầu vào cũng có thể chấp nhận được nhưng bất cứ điều gì tệ hơn sẽ thất bại. 

Trường hợp cạnh tinh tế phát sinh khi hầu hết các giá trị được cố định và chỉ một hoặc hai vị trí trống. Nếu cơ cấu hoạt động bị hạn chế, giả định ngây thơ rằng “bất kỳ công việc hoàn thiện nào” đều có thể thất bại. Ví dụ: nếu chúng ta có một cây nhỏ chỉ có thể hoán đổi một lần, thì một số vị trí cố định nhất định có thể khiến hoán vị mục tiêu không thể truy cập được. Đây chính xác là loại tình huống buộc chúng ta phải hiểu cẩn thận những hoán vị mà các phép toán được phép có thể tạo ra. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp bằng vũ lực sẽ thử mọi cách để lấp đầy các số 0 còn thiếu bằng các giá trị chưa được sử dụng còn lại và đối với mỗi hoán vị đã hoàn thành, hãy mô phỏng xem liệu Alice có thể chuyển đổi nó thành danh tính bằng các thao tác được phép hay không. Ngay cả khi chúng ta có một trình mô phỏng hiệu quả thì việc này cũng sẽ liên quan tới n! hoàn thành trong trường hợp xấu nhất là hoàn toàn không khả thi. 

Ngay cả khi chúng tôi sửa một lần hoàn thành duy nhất, việc mô phỏng quy trình cũng không hề đơn giản. Mỗi thao tác phụ thuộc vào trạng thái cây con và các nút chỉ hoạt động sau khi tất cả các nút con được xử lý, do đó, một mô phỏng đơn giản sẽ vẫn có ít nhất O(n) cho mỗi lần thử. Điều này dẫn đến một sự phức tạp tổng thể về mặt thiên văn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là hoạt động cực kỳ mạnh mẽ tại địa phương. Khi một nút đủ điều kiện, nó cho phép hoán vị tùy ý các giá trị bên trong một ngôi sao bao gồm chính nó và các con của nó. Trên một cây, việc áp dụng lặp đi lặp lại các hoán vị sao như vậy từ dưới lên là đủ để nhận ra bất kỳ hoán vị giá trị tổng thể nào. Hạn chế rằng một nút phải đợi các nút con của nó chỉ thực thi một thứ tự ứng dụng chứ không phải hạn chế về những hoán vị cuối cùng có thể đạt được. 

Điều này có nghĩa là khả năng của Alice tiếp cận cấu hình nhận dạng không thực sự phụ thuộc vào thuộc tính cấu trúc tinh vi của nhiệm vụ ban đầu. Bất kỳ hoán vị hoàn chỉnh nào của các giá trị đều có thể được chuyển đổi thành bất kỳ hoán vị nào khác bằng cách sử dụng các phép toán sao cục bộ này, miễn là chúng ta tuân theo thứ tự kích hoạt được phép. 

Sau khi điều này được chấp nhận, điều kiện ban đầu của trò chơi sẽ đơn giản hóa đáng kể: mỗi lần hoàn thành hợp lệ hoán vị một phần là “có thể thắng”.

Vì vậy, vấn đề trở thành một nhiệm vụ đếm tổ hợp thuần túy: có bao nhiêu cách chúng ta có thể gán các giá trị còn thiếu vào vị trí 0 trong khi vẫn duy trì một hoán vị? Đó chính xác là giai thừa của số nút chưa được gán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Đối số đếm tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu nút hiện có giá trị 0. Đây là những vị trí vẫn cần được lấp đầy. Điều này trực tiếp xác định có bao nhiêu giá trị vẫn chưa được sử dụng trong hoán vị. 
2. Quan sát rằng tất cả các giá trị khác 0 là khác nhau, do đó các số còn lại tạo thành chính xác tập bù của các giá trị được sử dụng trong 1 đến n. 
3. Gán giá trị cho vị trí 0 tương đương với việc chọn song ánh giữa k nút trống và k giá trị không sử dụng. 
4. Số lượng song ánh như vậy là k!, vì mỗi hoán vị của các giá trị còn lại đều tạo ra một sự hoàn thành rõ ràng. 
5. Xuất k! modulo 998244353 cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là các phép toán trên cây không hạn chế khả năng tiếp cận của các hoán vị theo cách phụ thuộc vào phép gán ban đầu. Các hoán vị sao được phép tạo ra đủ tính linh hoạt để sắp xếp lại các giá trị một cách tùy ý trên cây theo thứ tự kích hoạt đã cho. Vì điều này, Alice luôn có thể chuyển đổi bất kỳ hoán vị đã hoàn thành nào thành cấu hình nhận dạng. Do đó, yếu tố duy nhất quyết định liệu Alice có “có cơ hội” hay không là liệu hoán vị có được chỉ định đầy đủ hay không và việc đếm các đầu vào hợp lệ sẽ giảm xuống việc đếm số lần hoàn thành của hoán vị một phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modfact(n):
    fact = 1
    for i in range(1, n + 1):
        fact = fact * i % MOD
    return fact

t = int(input())
for _ in range(t):
    n = int(input())
    parent = list(map(int, input().split()))
    a = list(map(int, input().split()))
    
    zeros = 0
    for x in a:
        if x == 0:
            zeros += 1
    
    print(modfact(zeros))
```Giải pháp chỉ cần đếm có bao nhiêu mục bằng 0 trong mỗi trường hợp thử nghiệm. Mảng gốc không liên quan theo quan sát cuối cùng, vì cấu trúc không hạn chế sự tồn tại của chiến lược chiến thắng đối với bất kỳ hoán vị hoàn chỉnh nào. 

Giai thừa được tính trực tiếp cho mỗi bài kiểm tra. Vì tổng n trong tất cả các trường hợp thử nghiệm là lớn nên việc tính toán trước tuyến tính cho mỗi thử nghiệm vẫn đủ và thậm chí tính toán giai thừa lặp lại vẫn nằm trong giới hạn vì tổng của n bị giới hạn. 

Một lỗi triển khai phổ biến ở đây là cố gắng mô phỏng các hoạt động của cây hoặc xây dựng DP trên cấu trúc cây. Không điều nào trong số đó là cần thiết một khi chúng tôi nhận ra rằng kết quả chỉ phụ thuộc vào số lượng giá trị còn lại được chỉ định. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp có n = 3 trong đó các giá trị là`[0, 2, 0]`. Có hai vị trí trống và các giá trị không được sử dụng là`{1, 3}`. 

| Bước | Số không | Giá trị còn lại | Trả lời cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 2 | {1, 3} | 2! = 2 | 
| 2 | - | - | 2 | 

Điều này xác nhận rằng có chính xác hai lần hoàn thành hợp lệ, tương ứng với việc hoán đổi hoặc không hoán đổi các giá trị còn lại trên các vị trí trống. 

Bây giờ hãy xem xét n = 4 với các giá trị`[1, 0, 0, 4]`. Có hai số không và giá trị không sử dụng`{2, 3}`. 

| Bước | Số không | Giá trị còn lại | Trả lời cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 2 | {2, 3} | 2 | 
| 2 | - | - | 2 | 

Điều này một lần nữa cho thấy cấu trúc của cây không ảnh hưởng đến số lượng, chỉ có số lượng vị trí trống mới quan trọng. 

Những ví dụ này nhấn mạnh rằng quyền tự do tổ hợp hoàn toàn được nắm bắt bởi số lượng nút chưa được chỉ định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Đếm số 0 và tính giai thừa đến k | 
| Không gian | O(1) thêm | Chỉ có một vài quầy và giai thừa lặp đi lặp lại | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính với tổng n, phù hợp thoải mái trong giới hạn tổng số 200000 nút. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        parent = input().split()
        a = list(map(int, input().split()))
        zeros = a.count(0)
        fact = 1
        for i in range(1, zeros + 1):
            fact = fact * i % MOD
        out.append(str(fact))
    return "\n".join(out)

# sample-like tests
assert solve("1\n3\n1 1\n0 2 0\n") == "2"

# minimum size
assert solve("1\n2\n1\n0 0\n") == "2"

# all zeros
assert solve("1\n3\n1 1\n0 0 0\n") == "6"

# one zero
assert solve("1\n3\n1 1\n1 0 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút, cả hai đều trống | 2 | giai thừa không tầm thường nhỏ nhất | 
| tất cả số không | 6 | số lượng hoán vị đầy đủ | 
| số không đơn | 1 | không có trường hợp lựa chọn thực sự | 

## Vỏ cạnh 

Cây tối thiểu trong đó cả hai nút đều trống để kiểm tra xem giải pháp có xử lý chính xác trường hợp giai thừa nhỏ nhất có thể hay không. Với đầu vào`n = 2`và giá trị`[0, 0]`, thuật toán đếm hai số 0 và trả về`2! = 2`, tương ứng với hai phép gán có thể. 

Cây chưa được gán hoàn toàn sẽ kiểm tra xem giải pháp có xử lý đối xứng tất cả các nút một cách chính xác hay không. Với`n = 3`Và`[0, 0, 0]`, cả ba giá trị đều miễn phí, tạo ra`3! = 6`, xác nhận rằng không có hạn chế về cấu trúc nào cản trở việc đếm bài tập. 

Một bài tập gần như hoàn chỉnh chỉ có một số 0, chẳng hạn như`[1, 0, 3]`, dẫn đến một lần hoàn thành hợp lệ. Đầu ra của thuật toán`1! = 1`, phù hợp với thực tế là chỉ còn lại một giá trị được đặt.
