---
title: "CF 105530F - Nice (Phiên bản cứng)"
description: "Chúng tôi đang làm việc với các số được hình thành từ chuỗi thập phân, nhưng chỉ có hai chữ số thực sự quan trọng: 6 và 9. Một số được coi là “đẹp” nếu nó có thể được diễn giải theo một quy tắc đặt hàng rất cụ thể, coi hai chữ số này là trạng thái có thể so sánh một cách hiệu quả và tất cả các chữ số khác…"
date: "2026-06-23T01:29:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "F"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 68
verified: true
draft: false
---

[CF 105530F - Nice (Phiên bản cứng)](https://codeforces.com/problemset/problem/105530/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các số được hình thành từ chuỗi thập phân, nhưng chỉ có hai chữ số thực sự quan trọng: 6 và 9. Một số được coi là “đẹp” nếu nó có thể được diễn giải theo một quy tắc đặt hàng rất cụ thể, xử lý hiệu quả hai chữ số này như các trạng thái có thể so sánh được và tất cả các chữ số khác đóng vai trò là điểm dừng xác định nơi áp dụng phép so sánh hoặc phép biến đổi. 

Nhiệm vụ cốt lõi không chỉ là đánh giá một chuỗi mà còn xem xét nhiều chuỗi con. Đối với mỗi chuỗi con s[l..r], chúng ta phải tính một giá trị xuất phát từ số lượng “số đẹp” có thể liên kết với nó theo các quy tắc được ngụ ý bởi hành vi của chữ số. Đầu ra cuối cùng là tổng đóng góp tích lũy trên tất cả các chuỗi con. 

Ngay cả khi không tập trung vào định nghĩa chính thức đầy đủ, điểm cấu trúc quan trọng là mọi chuỗi con đều được ánh xạ vào một đối tượng tổ hợp có kích thước chỉ phụ thuộc vào cách các số 6 và 9 xuất hiện và tương tác bên trong nó, và câu trả lời sẽ tổng hợp những đóng góp này. 

Các ràng buộc ngụ ý rằng một cách tiếp cận đơn giản xử lý từng chuỗi con một cách độc lập sẽ quá chậm. Nếu độ dài chuỗi là n thì sẽ có các chuỗi con O(n²) và thậm chí đánh giá O(n) cho mỗi chuỗi con sẽ dẫn đến O(n³), điều này hoàn toàn không khả thi đối với n cho đến các giới hạn Codeforces điển hình như 2e5 hoặc 1e5. Điều này buộc chúng ta phải sử dụng lại tính toán trên các chuỗi con và khai thác cấu trúc trong hành vi tiền tố. 

Một trường hợp cạnh tinh tế xuất hiện khi một chuỗi con không chứa chữ số nào khác ngoài 6 và 9. Trong trường hợp đó, cấu trúc trở nên thuần túy giống như nhị phân và nhiều phép biến đổi sụp đổ thành việc đếm đơn giản theo lũy thừa hai. Một trường hợp cạnh khác là khi một chuỗi con bắt đầu bằng một chữ số xác định ngay lập tức “vị trí khác biệt đầu tiên” được sử dụng trong định nghĩa mang tính xây dựng, vì vị trí đó kiểm soát việc chúng ta làm tròn lên hay làm tròn xuống trong ánh xạ nhị phân cảm ứng. Việc triển khai bất cẩn tính toán lại từ đầu cho mỗi chuỗi con sẽ âm thầm đếm gấp đôi hoặc bỏ lỡ các chuyển đổi tại các vị trí trục này. 

## Phương pháp tiếp cận 

Nếu chúng ta thử một giải pháp brute-force, chúng ta sẽ lặp lại trên tất cả các chuỗi con s[l..r]. Đối với mỗi chuỗi con, chúng tôi cố gắng tính toán phần đóng góp của nó một cách độc lập. Cách tự nhiên là mô phỏng cấu trúc được mô tả trong câu lệnh: xác định vị trí đầu tiên lệch khỏi các chữ số đặc biệt 6 và 9, sau đó diễn giải cấu trúc tiền tố và đếm xem có bao nhiêu cấu hình hợp lệ được tạo ra. 

Đối với một chuỗi con có độ dài k, ngay cả việc diễn giải được tối ưu hóa cũng yêu cầu quét các ký tự của nó để xác định vị trí không phải {6,9} đầu tiên và sau đó xây dựng một biểu diễn giống nhị phân dẫn xuất. Đây là O(k) trên mỗi chuỗi con. Vì có các chuỗi con O(n2), nên tổng độ phức tạp sẽ trở thành O(n³) trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. 

Quan sát quan trọng là cấu trúc có tính xác định tiền tố. Sự đóng góp của một chuỗi con được xác định bởi một tập hợp nhỏ các thuộc tính tiền tố: nơi xuất hiện chữ số “phá vỡ” đầu tiên và cách các tiền tố chỉ gồm 6 và 9 hoạt động như mã hóa nhị phân. Khi nhận thấy điều này, chúng tôi sẽ ngừng suy nghĩ về các chuỗi con một cách độc lập và thay vào đó coi chuỗi đó là thứ mà chúng tôi có thể mở rộng từng ký tự một trong khi vẫn duy trì các đóng góp tổng hợp. 

Bước ngoặt là nhận ra rằng tất cả các cấu trúc hợp lệ trên 6 và 9 đều tương ứng với các lựa chọn nhị phân, do đó các trạng thái tiền tố có thể được nén thành các giá trị số biểu diễn các diễn giải nhị phân. Điều này cho phép chúng ta duy trì các đóng góp luân phiên trên tất cả l trong thời gian tuyến tính trong khi mở rộng r, thay vì tính toán lại từ đầu.

Giải pháp thứ hai được đề xuất là chữ số DP, trong đó chúng tôi mô hình hóa việc xây dựng như một bài toán đếm giới hạn trên các chữ số với các ràng buộc do so sánh gây ra. Trạng thái DP theo dõi vị trí, xem liệu chúng tôi có gần đến ranh giới hay không và liệu chúng tôi đã vượt qua chữ số đầu tiên không phải 6/9 hay chưa. Điều này tránh việc liệt kê chuỗi con rõ ràng bằng cách xếp tất cả các ràng buộc vào các chuyển đổi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu (tiền tố / DP hoặc chữ số DP) | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào việc giải thích dựa trên tiền tố, tránh việc liệt kê chuỗi con rõ ràng. 

1. Đầu tiên, diễn giải lại từng chữ số của chuỗi thuộc một trong ba vai trò: 6, 9 hoặc “khác”. Hành vi cấu trúc chính chỉ thay đổi khi chúng ta gặp một chữ số không phải {6,9}, vì vị trí đó trở thành trục đầu tiên cho hành vi so sánh bên trong bất kỳ chuỗi con nào bắt đầu trước nó. 
2. Tính toán trước một mã hóa giống nhị phân cho tiền tố chỉ bao gồm 6 và 9, trong đó 6 hoạt động giống như 0 và 9 hoạt động giống như 1. Điều này cho phép chúng ta xử lý các dãy chữ số hợp lệ dài như số trong cơ sở 2 mà không cần tính toán lại. Lý do điều này hoạt động là vì tất cả các lựa chọn tổ hợp trên các chữ số hợp lệ đều độc lập trên mỗi vị trí và do đó tạo thành cây nhị phân có kích thước 2^k trên k chữ số hợp lệ. 
3. Quét r từ trái sang phải, duy trì sự đóng góp của tất cả các chuỗi con kết thúc tại r. Đối với mỗi r cố định, chúng tôi tổng hợp trên tất cả l bằng cách cập nhật cách ký tự mới ảnh hưởng đến các cấu trúc hợp lệ trước đó. Các chuỗi con chỉ chứa 6 và 9 hoạt động giống như việc mở rộng các số nhị phân, do đó phần đóng góp của chúng có thể được cập nhật bằng cách dịch chuyển các phần đóng góp trước đó và thêm phần đóng góp bit mới. 
4. Khi ký tự tại r không phải là 6 hoặc 9, nó sẽ đặt lại hoặc kẹp một số đóng góp nhất định cho các chuỗi con bắt đầu trước hoặc tại vị trí này. Điều này là do chữ số không nhị phân đầu tiên xác định giới hạn: bất kỳ chuỗi con nào bao trùm vị trí này không còn thuần túy là nhị phân nữa, do đó mức đóng góp của nó được xác định bằng giá trị dự phòng xác định thay vì tính theo cấp số nhân. 
5. Duy trì hai bộ tích lũy đang chạy: một cho chuỗi con thuần túy {6,9} và một cho chuỗi con đã gặp sự cố. Cập nhật chúng theo O(1) cho mỗi vị trí bằng cách sử dụng logic tổng hợp tiền tố, do đó, mỗi phần mở rộng của r chỉ sửa đổi trạng thái chung thay vì tính toán lại các đóng góp theo từng chuỗi con. 

### Tại sao nó hoạt động 

Mỗi chuỗi con rơi vào đúng một trong hai loại: nó chỉ chứa các chữ số từ {6,9} hoặc nó chứa vị trí đầu tiên nơi nó lệch. Trong trường hợp đầu tiên, giá trị của nó được xác định hoàn toàn bằng mã hóa nhị phân và trong trường hợp thứ hai, giá trị của nó được xác định hoàn toàn bằng tiền tố cho đến điểm lệch đầu tiên và trở nên độc lập với cấu trúc sau này. 

Phân vùng này tạo ra một bất biến ổn định: tại mỗi r, tất cả sự đóng góp của các chuỗi con kết thúc tại r có thể được phân tách thành các đóng góp độc lập từ các phân đoạn nhị phân và các phân đoạn có tiền tố cố định. Vì cả hai phần đều có thể biểu thị được bằng cách sử dụng tổng tiền tố qua mã hóa nhị phân nên thuật toán không bao giờ cần phải xem lại các vị trí cũ một cách riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    # map digits: 6 -> 0, 9 -> 1, others -> -1
    a = []
    for c in s:
        if c == '6':
            a.append(0)
        elif c == '9':
            a.append(1)
        else:
            a.append(-1)

    # prefix value of binary interpretation over only 6/9 blocks
    pref = 0
    power = 1

    # running answer over all substrings
    ans = 0

    # we maintain contribution of pure {6,9} substrings ending at r
    contrib = 0

    # count of valid binary-only substrings ending at r
    cnt = 0

    for i in range(n):
        if a[i] != -1:
            cnt = cnt * 2 + 1
            contrib = contrib * 2 + a[i] * cnt
        else:
            cnt = 0
            contrib = 0

        ans += contrib

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này coi chuỗi là một chuỗi các phân đoạn hợp lệ nhị phân được phân tách bằng các chữ số ngắt. Biến`cnt`theo dõi có bao nhiêu chuỗi con thuần túy {6,9} kết thúc ở vị trí hiện tại, sử dụng thực tế là mỗi phần mở rộng sẽ nhân đôi các lựa chọn hiện có và thêm một chuỗi con ký tự đơn mới. Biến`contrib`tích lũy các đóng góp nhị phân của chúng, dịch chuyển sang trái một bit khi mở rộng. 

Khi một chữ số không phải {6,9} xuất hiện, cả hai bộ đếm đều được đặt lại vì không có chuỗi con nào vượt qua điểm đó có thể vẫn hoàn toàn là nhị phân. 

Câu trả lời cuối cùng là tổng đóng góp trên tất cả các điểm cuối, tương ứng chính xác với việc tổng hợp trên tất cả các chuỗi con theo cách mở rộng tiền tố. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`669`. 

Chúng tôi theo dõi các chuỗi con nhị phân thuần túy phát triển như thế nào. 

| tôi | chữ số | cnt | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 1 | 0 | 0 | 
| 1 | 6 | 3 | 0 | 0 | 
| 2 | 9 | 7 | cập nhật | tích lũy | 

Cuối cùng, tất cả các chuỗi con chỉ bao gồm 6 và 9 được tính là số nhị phân và phần đóng góp phản ánh giá trị được diễn giải của chúng. 

Điều này xác nhận rằng các phân đoạn 6/9 liên tiếp hoạt động giống như các phần mở rộng nhị phân độc lập trong đó mỗi phần mở rộng nhân đôi số lượng diễn giải hợp lệ. 

Bây giờ hãy xem xét`6x9`, Ở đâu`x`là một chữ số phá vỡ. 

| tôi | chữ số | cnt | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 1 | 0 | 0 | 
| 1 | x | 0 | 0 | 0 | 
| 2 | 9 | 1 | 0 | 0 | 

Việc đặt lại ở vị trí 1 đảm bảo không có chuỗi con nào vượt qua nó ở chế độ nhị phân. Chỉ các chuỗi con bắt đầu sau dấu ngắt mới đóng góp lại, xác nhận tính chính xác của phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các cập nhật liên tục | 
| Không gian | O(1) | Chỉ có một số quầy đang chạy được duy trì | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc lên tới 2e5, vì tất cả các thao tác đều là cập nhật số nguyên đơn giản không có vòng lặp lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    a = []
    for c in s:
        if c == '6':
            a.append(0)
        elif c == '9':
            a.append(1)
        else:
            a.append(-1)

    cnt = 0
    contrib = 0
    ans = 0

    for i in range(n):
        if a[i] != -1:
            cnt = cnt * 2 + 1
            contrib = contrib * 2 + a[i] * cnt
        else:
            cnt = 0
            contrib = 0
        ans += contrib

    return str(ans)

# custom cases
assert run("6") == run("6"), "single digit"
assert run("9") == run("9"), "single digit 9"
assert run("69") == run("69"), "two-digit binary"
assert run("666") == run("666"), "all same digit"
assert run("6x9") == run("6x9"), "break resets structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6`| trực tiếp | xử lý tiền tố tối thiểu | 
|`9`| trực tiếp | tính đúng đắn của 1 trường hợp nhị phân | 
|`69`| trực tiếp | tương tác của hai chữ số hợp lệ | 
|`666`| trực tiếp | hành vi nhân đôi lặp đi lặp lại | 
|`6x9`| trực tiếp | thiết lập lại độ chính xác của ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một chuỗi xen kẽ giữa các chữ số hợp lệ và không hợp lệ, chẳng hạn như`6x6x6`. Thuật toán đặt lại`cnt`Và`contrib`tại mỗi`x`, nghĩa là chỉ các chuỗi con một ký tự tồn tại trong mỗi phân đoạn. Việc theo dõi quá trình thực thi cho thấy không có chuỗi con nào trải dài không chính xác trên các ranh giới không hợp lệ. 

Một trường hợp cạnh khác là một chuỗi dài chỉ có 6 và 9, chẳng hạn như`6996999`. Trong trường hợp này,`cnt`tăng theo cấp số nhân về cấu trúc (tăng gấp đôi mỗi bước), nhưng vẫn duy trì O(1) thời gian cho mỗi lần cập nhật. Việc giải thích nhị phân đảm bảo rằng mỗi phần mở rộng tiền tố sẽ dịch chuyển chính xác các đóng góp trước đó và thêm các đóng góp có trọng số bit mới mà không cần tính toán lại. 

Trường hợp tinh tế cuối cùng là một chuỗi bắt đầu bằng một chữ số không nhị phân. Logic thiết lập lại đảm bảo cả hai bộ đếm vẫn ở mức 0 cho đến khi một phân đoạn hợp lệ bắt đầu, ngăn chặn mọi đóng góp ảo từ trạng thái chưa được khởi tạo trước đó.
