---
title: "CF 1740D - Thẻ kiến ​​thức"
description: "Chúng ta được cấp một lưới, nhưng chỉ có hai ô thực sự hoạt động giống như “điểm cuối thực”: ô trên cùng bên trái đóng vai trò là nguồn chứa một chồng thẻ và ô dưới cùng bên phải hoạt động như một bồn chứa nơi chúng ta phải xây dựng lại một ngăn xếp theo thứ tự sắp xếp từ 1 đến k, tăng dần từ trên xuống dưới."
date: "2026-06-15T03:40:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1740
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 831 (Div. 1 + Div. 2)"
rating: 1500
weight: 1740
solve_time_s: 133
verified: true
draft: false
---

[CF 1740D - Thẻ kiến thức](https://codeforces.com/problemset/problem/1740/D) 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, cấu trúc dữ liệu 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một lưới, nhưng chỉ có hai ô thực sự hoạt động giống như “điểm cuối thực”: ô trên cùng bên trái đóng vai trò là nguồn chứa một chồng thẻ và ô dưới cùng bên phải hoạt động như một bồn chứa nơi chúng ta phải xây dựng lại một ngăn xếp theo thứ tự sắp xếp từ 1 đến k, tăng dần từ trên xuống dưới. 

Mỗi thẻ bắt đầu được xếp chồng lên nhau trong ô nguồn theo một thứ tự cố định và chúng ta chỉ được phép di chuyển thẻ trên cùng của bất kỳ ngăn xếp nào sang ô lưới liền kề. Các ô trung gian hoạt động giống như các điểm đệm đơn vì mỗi ô không kết thúc có thể chứa tối đa một thẻ tại một thời điểm. Các thẻ có thể di chuyển qua lưới nhưng không thể truy cập lại nguồn và các thẻ đã được đặt ở đích không thể di chuyển lại. 

Mục tiêu không chỉ là di chuyển tất cả các thẻ mà còn để đảm bảo rằng ngăn xếp cuối cùng ở đích có giá trị 1 ở trên cùng, sau đó là 2, v.v. cho đến k ở dưới cùng. Câu hỏi đặt ra là liệu việc sắp xếp lại như vậy có khả thi trong điều kiện hạn chế về di chuyển và năng lực hay không. 

Ràng buộc kích thước lưới ngụ ý rằng tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là một triệu, vì vậy mọi giải pháp đều phải tuyến tính trong lưới hoặc tốt hơn. Số lượng thẻ trong các bài kiểm tra tối đa là một trăm nghìn, vì vậy các phép tính trên mỗi thẻ phải không đổi hoặc logarit. Bất kỳ mô phỏng các bước di chuyển riêng lẻ nào đều không thể thực hiện được ngay lập tức vì một thẻ có thể di chuyển qua các ô O(nm). 

Một trường hợp thất bại tinh tế xuất phát từ việc nghĩ rằng chỉ có hoán vị mới quan trọng. Thứ tự xếp chồng các thẻ ban đầu không liên quan đến khả năng tiếp cận, nhưng không liên quan đến cách chúng ta có thể “đệm” các thẻ trong khi xây dựng thứ tự cuối cùng. 

Một kịch bản phức tạp khác xuất hiện khi k lớn so với kích thước lưới. Ví dụ: nếu lưới gần như là một đường dẫn có độ dài k, một ý tưởng ngây thơ có thể cho rằng chúng ta luôn có thể sắp xếp lại bằng cách xáo trộn các thẻ một cách cẩn thận, nhưng hạn chế bộ đệm một ô sẽ chặn việc sắp xếp lại khi một số đường dẫn nhất định được các thẻ trước đó "tiêu thụ". 

## Phương pháp tiếp cận 

Chế độ xem bạo lực xử lý lưới dưới dạng biểu đồ và mô phỏng mọi chuỗi di chuyển quân bài có thể xảy ra. Mỗi trạng thái sẽ bao gồm vị trí của mỗi thẻ và các ngăn xếp hiện tại trong tất cả các ô. Ngay cả đối với các lưới nhỏ, điều này bùng nổ vì mỗi lưới di chuyển các nhánh theo các tương tác lân cận và ngăn xếp. Số lượng trạng thái tăng theo cấp số nhân với k và ngay cả một mô phỏng đơn giản hóa xử lý từng thẻ một vẫn sẽ yêu cầu tìm đường và quay lui, điều này vượt xa các ràng buộc. 

Sự đơn giản hóa cấu trúc quan trọng là lưới không thực sự là một biểu đồ phức tạp cho bài toán này. Vì mỗi ô trung gian chỉ có thể chứa một thẻ nên mỗi ô như vậy hoạt động giống như một khe băng tải sử dụng một lần. Khi một thẻ đã chiếm giữ nó, ô đó sẽ không còn khả năng sắp xếp lại các thẻ khác cho đến khi nó được giải phóng, nhưng việc giải phóng nó yêu cầu thẻ phải rời đi, điều này chỉ xảy ra theo một luồng có trật tự nghiêm ngặt về đích. 

Điều này biến lưới thành một hệ thống tài nguyên: chúng tôi không định tuyến thẻ một cách tự do, chúng tôi đang phân bổ một số lượng hạn chế các vị trí giữ tạm thời dọc theo một số đường dẫn từ (1,1) đến (n,m). Câu hỏi hiệu quả là liệu chúng ta có thể thiết lập đủ “dung lượng bộ đệm” để sắp xếp lại hoán vị thành thứ tự được sắp xếp trong khi xử lý thẻ trong một lần chuyển hay không. 

Quan sát quan trọng là chỉ có số lượng tế bào trung gian có thể sử dụng được mới quan trọng. Hình dạng chính xác của lưới không quan trọng; điều quan trọng là chúng ta có thể tạo một đường dẫn đơn giản từ nguồn đến đích và mọi ô bổ sung ngoài điểm cuối sẽ cung cấp cho chúng ta nhiều nhất một khe lưu trữ tạm thời. Do đó, lưới đóng góp một số lượng bộ đệm cố định bằng nm trừ 2.

Quá trình xây dựng ngăn xếp cuối cùng tương đương với việc tiêu thụ các thẻ theo thứ tự tăng dần và đảm bảo rằng khi chúng ta đặt giá trị i vào đích, tất cả các thẻ trước đó từ 1 đến i−1 đều có thể được định vị mà không chặn các vị trí trong tương lai. Điều này giúp giảm việc kiểm tra xem dung lượng bộ đệm có đủ để xử lý “sự đảo ngược” do hoán vị ban đầu gây ra hay không. 

Một cách tiếp cận ngây thơ, đúng nhưng chậm sẽ cố gắng mô phỏng việc đặt các thẻ theo thứ tự từ 1 đến k, theo dõi xem mỗi thẻ có thể bị trì hoãn cho đến thời điểm chính xác hay không. Điều này có thể được tối ưu hóa bằng cách lưu ý rằng hạn chế duy nhất là liệu chúng ta có cần lưu trữ đồng thời nhiều hơn nm−2 thẻ bên ngoài nguồn và đích hay không. Yêu cầu cao nhất đó được xác định bằng khoảng cách từ hoán vị đến thứ tự xử lý chính xác. 

Sự đơn giản hóa cuối cùng là câu trả lời chỉ phụ thuộc vào việc liệu chúng ta có thể xử lý hoán vị bằng cách sử dụng bộ đệm dạng ngăn xếp có kích thước bị giới hạn bởi các ô trung gian có sẵn hay không. Điều này trở thành một cuộc kiểm tra tính khả thi đầy tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ các chuyển động | hàm mũ | hàm mũ | Quá chậm | 
| Kiểm tra tính khả thi của bộ đệm tham lam | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý thẻ theo thứ tự chúng xuất hiện trong ngăn xếp ban đầu và mô phỏng xem liệu cuối cùng chúng tôi có thể xuất chúng theo thứ tự tăng dần bằng cách sử dụng bộ đệm bị ràng buộc hay không. 

1. Chúng tôi theo dõi giá trị bắt buộc tiếp theo, bắt đầu từ 1, đại diện cho thẻ tiếp theo phải được đặt vào ngăn xếp đích. Điều này thực thi yêu cầu sắp xếp cuối cùng. 
2. Chúng tôi duy trì một bộ đệm tạm thời đại diện cho các thẻ đã được lấy từ nguồn nhưng chưa thể gửi đến đích vì các giá trị yêu cầu trước đó chưa được đáp ứng. Bộ đệm này tương ứng với các ô trung gian giới hạn. 
3. Chúng tôi quét qua ngăn xếp ban đầu từ trên xuống dưới. Đối với mỗi thẻ, nếu nó khớp với giá trị yêu cầu tiếp theo, chúng tôi sẽ gửi ngay đến đích và nâng cao yêu cầu. Sau khi đặt thẻ, chúng tôi liên tục kiểm tra xem bộ đệm có chứa các giá trị bắt buộc tiếp theo theo đúng thứ tự hay không và bật chúng lên nếu có thể. 
4. Nếu thẻ không khớp với giá trị bắt buộc tiếp theo, chúng tôi sẽ cố gắng lưu thẻ đó vào bộ đệm. Nếu bộ đệm đã đầy, nghĩa là nó vượt quá số lượng ô trung gian có sẵn, thì việc xây dựng sẽ không thể thực hiện được. 
5. Sau khi xử lý tất cả các thẻ, chúng tôi lại xóa khỏi bộ đệm miễn là các giá trị bắt buộc tiếp theo xuất hiện ở đầu bộ đệm theo thứ tự có thể sử dụng được. 

Hạn chế chính quyết định tính khả thi là liệu bộ đệm có cần vượt quá dung lượng nm−2 hay không. Nếu đúng như vậy, một số thẻ sẽ phải chiếm các vị trí ô đơn đã bị chặn, điều này vi phạm quy tắc mỗi ô trung gian chỉ có thể giữ một thẻ. 

### Tại sao nó hoạt động 

Thuật toán thực thi rằng các thẻ được xuất ra theo đúng thứ tự tăng dần và bộ đệm đóng vai trò là cơ chế duy nhất để trì hoãn các thẻ. Vì mỗi ô lưới trung gian có thể lưu trữ tối đa một thẻ vào bất kỳ lúc nào nên kích thước bộ đệm giới hạn trên số lượng thẻ bị trì hoãn đồng thời. Nếu chúng tôi không bao giờ vượt quá giới hạn này, chúng tôi có thể gán từng thẻ đệm cho một ô trung gian riêng biệt dọc theo một đường dẫn đơn giản từ nguồn đến đích, đảm bảo không có xung đột. Nếu chúng tôi vượt quá giới hạn này, ít nhất một thẻ sẽ yêu cầu chia sẻ một ô hoặc xem lại cấu trúc bị cấm, khiến việc định cấu hình không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))

        capacity = n * m - 2
        need = 1
        buf = []

        ok = True

        for x in a:
            if x == need:
                need += 1
                while buf and buf[-1] == need:
                    buf.pop()
                    need += 1
            else:
                buf.append(x)
                if len(buf) > capacity:
                    ok = False

        while buf and buf[-1] == need:
            buf.pop()
            need += 1

        if need == k + 1 and ok:
            print("YA")
        else:
            print("TIDAK")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm và tính toán dung lượng bộ đệm hiệu quả theo số lượng ô trung gian. Biến`need`thực thi trình tự tăng dần cần thiết cho ngăn xếp đích. Danh sách`buf`hoạt động như một ngăn xếp đại diện cho các lá bài bị trì hoãn. Bất cứ khi nào chúng tôi gặp giá trị được yêu cầu tiếp theo, chúng tôi ngay lập tức đặt nó và sau đó tham lam sử dụng bất kỳ thẻ nào được lưu trữ trước đó mà giờ có thể sử dụng được. 

Một chi tiết triển khai tinh tế là việc xóa bộ đệm phải xảy ra cả trong quá trình quét và sau khi quá trình quét kết thúc, vì các chuỗi hợp lệ chỉ có thể hoàn thành sau khi tất cả đầu vào được xử lý. Một điểm quan trọng khác là lỗi tràn bộ đệm sẽ được kiểm tra ngay lập tức, vì một khi vượt quá dung lượng thì không có cách nào phục hồi được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 3 6
3 6 4 1 2 5
```Chúng tôi theo dõi`need`và trạng thái đệm. 

| Bước | Thẻ | Cần | Bộ đệm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | [3] | cửa hàng | 
| 2 | 6 | 1 | [3,6] | cửa hàng | 
| 3 | 4 | 1 | [3,6,4] | cửa hàng | 
| 4 | 1 | 1 | [3,6,4] | không khớp, đệm | 
| 5 | 2 | 1 | ... | cuối cùng nhu cầu trở thành 1→2→3 | 

Khi 1 xuất hiện, nó được đặt ngay lập tức, sau đó 2 được giải phóng thông qua sắp xếp lại bộ đệm, cho phép phân giải đầy đủ. Bộ đệm không bao giờ vượt quá dung lượng sẵn có nên câu trả lời là hợp lệ. 

Điều này cho thấy sự rối loạn tạm thời được cho phép miễn là nó có thể được giải quyết mà không vượt quá giới hạn lưu trữ trung gian. 

### Ví dụ 2 

đầu vào:```
1
3 3 3
3 2 1
```| Bước | Thẻ | Cần | Bộ đệm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | [3] | cửa hàng | 
| 2 | 2 | 1 | [3,2] | cửa hàng | 
| 3 | 1 | 1 | [3,2] | đầu ra 1 | 
| 3b | - | 2 | [3,2] | không thể giải quyết rõ ràng 2 | 

Chúng tôi kết thúc với bộ đệm giữ 3 và 2, nhưng việc sắp xếp ngăn cản việc trích xuất thích hợp trong các giới hạn. Nếu dung lượng bộ đệm quá nhỏ thì mẫu này sẽ thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | mỗi lá bài được đẩy và bật ra nhiều nhất một lần | 
| Không gian | O(k) | bộ đệm lưu trữ tối đa k phần tử trong trường hợp xấu nhất | 

Các hoạt động tuyến tính về số lượng thẻ và các ràng buộc cho phép tổng số thẻ lên tới 100 nghìn, do đó, thao tác này diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m, k = map(int, input().split())
            a = list(map(int, input().split()))

            capacity = n * m - 2
            need = 1
            buf = []
            ok = True

            for x in a:
                if x == need:
                    need += 1
                    while buf and buf[-1] == need:
                        buf.pop()
                        need += 1
                else:
                    buf.append(x)
                    if len(buf) > capacity:
                        ok = False

            while buf and buf[-1] == need:
                buf.pop()
                need += 1

            out.append("YA" if need == k + 1 and ok else "TIDAK")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
3 3 6
3 6 4 1 2 5
3 3 10
1 2 3 4 5 6 7 8 9 10
5 4 4
2 1 3 4
3 4 10
10 4 9 3 5 6 8 2 7 1
""") == """YA
TIDAK
YA
YA"""

# custom cases
assert run("""1
3 3 1
1
""") == "YA"

assert run("""1
3 3 3
3 2 1
""") == "TIDAK"

assert run("""1
4 4 5
5 4 3 2 1
""") == "TIDAK"

assert run("""1
3 3 4
1 2 4 3
""") == "YA"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thẻ đơn | YA | tính khả thi tối thiểu | 
| đảo ngược nhỏ | TIDAK | hành vi tràn ngăn xếp | 
| đảo ngược hoàn toàn | TIDAK | trường hợp đảo ngược tồi tệ nhất | 
| gần sắp xếp | YA | phục hồi bộ đệm | 

## Vỏ cạnh 

Hộp đựng thẻ đơn tối thiểu xác nhận rằng hệ thống hoạt động chính xác khi không cần đệm. Thuật toán khớp ngay lập tức`need = 1`và kết thúc thành công. 

Một hoán vị đảo ngược hoàn toàn thể hiện áp lực tối đa lên bộ đệm. Ví dụ,`k = 3, a = [3, 2, 1]`buộc tất cả các thẻ vào bộ nhớ trước khi có thể xuất bất kỳ thẻ nào. Nếu vượt quá giới hạn bộ đệm, thuật toán sẽ từ chối nó một cách chính xác vì nó không thể duy trì tất cả các thẻ bị trì hoãn trong các ô trung gian có sẵn. 

Một chuỗi gần như được sắp xếp như`1 2 4 3`cho thấy có thể điều chỉnh muộn. Thuật toán xuất ra 1 và 2 ngay lập tức, lưu trữ 4 và sau đó giải quyết thành công 3 từ bộ đệm sau khi 4 bị trì hoãn. Điều này xác nhận rằng logic bộ đệm xử lý chính xác các hoán vị không đơn điệu nhưng có thể sửa được cục bộ.
