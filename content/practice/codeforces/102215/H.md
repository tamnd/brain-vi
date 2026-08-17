---
title: "CF 102215H - Thiếu số"
description: "Chúng ta có một mảng có độ dài n. Các phần tử của nó là các số nguyên riêng biệt từ 0 đến n, do đó không xuất hiện chính xác một giá trị trong phạm vi đó. Bản thân mảng bị ẩn. Chúng ta không thể đọc trực tiếp giá trị của nó."
date: "2026-08-17T23:48:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "H"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 552
verified: false
draft: false
---

[CF 102215H - Thiếu số](https://codeforces.com/problemset/problem/102215/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 12 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng có độ dài n. Các phần tử của nó là các số nguyên riêng biệt từ 0 đến n, do đó không xuất hiện chính xác một giá trị trong phạm vi đó. Bản thân mảng bị ẩn. Chúng ta không thể đọc trực tiếp giá trị của nó. Thay vào đó, đối với bất kỳ vị trí mảng i và vị trí bit b nào, chúng ta có thể yêu cầu bộ tương tác cung cấp bit thứ b của a i ​. 

Nhiệm vụ là xác định giá trị còn thiếu trong khi sử dụng tối đa các truy vấn 2n+19 bit. Tuyên bố chính thức xác nhận rằng sự tương tác bắt đầu bằng n, mỗi truy vấn trả về một bit và câu trả lời cuối cùng phải được in dưới dạng`! x`. 

Giới hạn n 1000 đủ nhỏ để việc tính toán thông thường trên tất cả các giá trị 0,…,n là rẻ. Ràng buộc khó khăn là giới hạn truy vấn. Việc đọc từng bit của mọi phần tử mảng sẽ sử dụng khoảng nlog 2​ n truy vấn, tức là khoảng 10000 truy vấn khi n=1000, vượt xa mức cho phép năm 2019. Thuật toán phải giảm số lượng câu hỏi chứ không chỉ đơn thuần là tối ưu hóa tính toán cục bộ. 

Có hai trường hợp đặc biệt thường gây ra lỗi. Đầu tiên, giá trị còn thiếu có thể là 0. Ví dụ: với n=1, mảng duy nhất có thể là`[1]`, vậy câu trả lời là`0`. Một giải pháp khởi tạo câu trả lời cho một giá trị khác 0 hoặc coi 0 là trường hợp lỗi đặc biệt có thể mắc sai lầm này. 

Thứ hai, giá trị còn thiếu có thể là n, kể cả khi bản thân n là lũy thừa của hai. Ví dụ: với n=4 và mảng`[0,1,2,3]`, câu trả lời là`4`. Vị trí bit 2 là cần thiết vì 4 là`100`ở dạng nhị phân. Một vòng lặp bất cẩn chỉ sử dụng`range(int(log2(n)))`sẽ dừng trước khi truy vấn bit đó và không bao giờ có thể phân biệt được 4 với các giá trị nhỏ hơn. 

Mảng được đảm bảo chứa các giá trị riêng biệt. Do đó, một đầu vào như`n=3`với`[1,1,2]`không phải là một trường hợp thử nghiệm hợp lệ. Cụ thể, mảng "tất cả đều bằng nhau" nằm ngoài miền của vấn đề, do đó, nó không thể được sử dụng một cách có ý nghĩa làm bài kiểm tra tính chính xác cho giải pháp tương tác được gửi. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ tái tạo lại hoàn toàn mọi phần tử mảng. Vì mọi giá trị đều nằm trong khoảng từ 0 đến n nên mỗi giá trị cần tối đa ⌊log 2 ​ n⌋+1 bit. Với n=1000, tức là 10 bit, vì vậy việc xây dựng lại tất cả 1000 phần tử yêu cầu 10000 truy vấn trong trường hợp xấu nhất. Khi đã biết các giá trị, việc tìm số còn thiếu là chuyện nhỏ nhưng ngân sách truy vấn đã vượt quá. 

Quan sát hữu ích là chúng ta không cần biết bất kỳ giá trị mảng hoàn chỉnh nào. Chúng ta chỉ cần biết các bit của một giá trị bị thiếu. 

Giả sử chúng ta đã biết một vài bit thấp của số còn thiếu. Hãy xem xét tất cả các giá trị từ 0 đến n có chính xác các bit đó. Gọi chúng là những giá trị có thể. Trong số các phần tử mảng thực tế, có chính xác một phần tử trong nhóm chứa giá trị còn thiếu, trong khi mọi nhóm còn lại đều có chính xác số phần tử như mong đợi. 

Bây giờ hãy truy vấn bit tiếp theo, nhưng chỉ đối với các vị trí mảng có các bit đã biết khớp với tiền tố bị thiếu. Chúng ta thu được hai nhóm, một nhóm có bit 0 và một nhóm có bit 1. Chúng ta cũng biết có bao nhiêu giá trị từ 0 đến n thuộc về mỗi nhóm. Nhóm có kích thước được quan sát nhỏ hơn kích thước dự kiến ​​của nó phải chứa giá trị còn thiếu. 

Lý do điều này lưu các truy vấn là sau khi sửa b bit thấp, các số có thể có cùng phần dư modulo 2 b. Bit tiếp theo của chúng chia chúng gần như đồng đều. Do đó, các vị trí mảng ứng viên sẽ co lại khoảng hai lần ở mỗi lần lặp. Chúng tôi không bao giờ truy vấn các vị trí đã bị loại bỏ. 

Lực lượng vũ phu hoạt động vì mỗi bit được truy vấn cung cấp đủ thông tin để xây dựng lại một phần tử, nhưng nó không thành công vì nó dành Θ(nlogn) truy vấn để xây dựng lại thông tin mà chúng ta không cần. Quan sát cho thấy chỉ nhóm chứa giá trị bị thiếu mới cho phép chúng ta đi theo một đường dẫn xuyên qua phân vùng nhị phân ẩn này, giảm tổng số truy vấn xuống O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(nlogn) | O(n) | Quá nhiều truy vấn | 
| Tối ưu | O(nlogn) tính toán cục bộ, truy vấn O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với mọi giá trị từ 0 đến n là giá trị có thể bị thiếu và mọi chỉ mục mảng là vị trí ứng cử viên. 

Tại thời điểm này, chúng ta không biết gì về số còn thiếu, vì vậy mọi giá trị và mọi phần tử mảng đều có liên quan. 
2. Xử lý các vị trí bit từ bit ít quan trọng nhất trở lên`n.bit_length() - 1`. 

Đây chính xác là các bit có thể xuất hiện trong một số từ 0 đến n. Việc xử lý các bit thấp trước tiên rất thuận tiện vì các giá trị có cùng các bit được xử lý tạo thành một cấp số cộng gần như đồng đều, mang lại thuộc tính thu gọn cần thiết. 
3. Chia các giá trị hiện tại có thể thành`possible_zero`Và`possible_one`theo bit hiện tại. 

Các danh sách này cho chúng ta biết có bao nhiêu giá trị cần có bit 0 và bit 1 nếu không thiếu giá trị nào. 
4. Đối với mọi chỉ mục mảng ứng cử viên hiện tại, hãy yêu cầu người tương tác cung cấp bit này và chia các chỉ mục thành`candidate_zero`Và`candidate_one`. 

Chúng tôi chỉ hỏi về các ứng cử viên có số bit được phát hiện trước đó phù hợp với số còn thiếu. Mọi phần tử mảng khác đã được loại trừ là nguồn của giá trị bị thiếu. 
5. So sánh quy mô nhóm được quan sát với quy mô nhóm dự kiến. 

Nếu như`len(candidate_zero) < len(possible_zero)`, giá trị bị thiếu có bit 0. Nếu không, nó có bit (1`, vì chính xác một giá trị không có trong nhóm chứa số bị thiếu. 
6. Chỉ giữ lại nhóm giá trị và nhóm chỉ số ứng viên tương ứng. 

Bất biến hiện được bảo tồn:`possible_values`chứa chính xác các giá trị phù hợp với tất cả các bit được phát hiện của số bị thiếu, trong khi`candidates`chứa chính xác các vị trí mảng có giá trị có cùng các bit đó. 
7. Đặt bit tương ứng của câu trả lời bất cứ khi nào nhóm được chọn là nhóm bit-1. 

Sau khi xử lý mọi bit có liên quan, chỉ còn lại một giá trị có thể. In nó với định dạng câu trả lời tương tác. 

### Tại sao nó hoạt động 

Ở đầu mỗi lần lặp, các vị trí mảng ứng cử viên biểu thị tất cả các giá trị hiện tại có cùng bit được xử lý với giá trị còn thiếu. tương ứng`possible_values`chứa tất cả các giá trị từ 0 đến n với cùng mẫu bit đó. Vì thiếu chính xác một giá trị nên mảng thực tế có ít hơn một phần tử so với mong đợi, chính xác trong nhóm chứa giá trị bị thiếu. So sánh kích thước nhóm được quan sát và dự kiến ​​sẽ xác định duy nhất bit bị thiếu. Lọc cả hai nhóm sau đó giữ nguyên bất biến cho bit tiếp theo. Sau khi tất cả các bit đã được xử lý, hai giá trị khác nhau không thể chia sẻ mọi bit liên quan, vì vậy giá trị có thể còn lại chính xác là số bị thiếu. 

## Giải pháp Python 

Vấn đề thực tế của Codeforces là có tính tương tác nên chương trình bên dưới phải được gửi dưới dạng giải pháp tương tác. Các dòng trống hiển thị trong mẫu của câu lệnh chỉ là định dạng trình bày và tương tác thực sự yêu cầu xóa sau mỗi truy vấn và sau câu trả lời cuối cùng.```python
Pythonimport sysinput = sys.stdin.readline

def main():    n = int(input())
    possible_values = list(range(n + 1))    candidates = list(range(n))
    answer = 0
    for bit in range(n.bit_length()):        possible_zero = []        possible_one = []
        for value in possible_values:            if (value >> bit) & 1:                possible_one.append(value)            else:                possible_zero.append(value)
        candidate_zero = []        candidate_one = []
        for idx in candidates:            print("?", idx + 1, bit, flush=True)            response = int(input())
            if response == -1:                sys.exit(0)
            if response == 0:                candidate_zero.append(idx)            else:                candidate_one.append(idx)
        if len(candidate_zero) < len(possible_zero):            possible_values = possible_zero            candidates = candidate_zero        else:            possible_values = possible_one            candidates = candidate_one            answer |= 1 << bit
    print("!", answer, flush=True)

if __name__ == "__main__":    main()
```Chương trình khởi tạo đầu tiên`possible_values`với tất cả các giá trị n+1 vì bất kỳ giá trị nào trong số chúng ban đầu có thể bị thiếu.`candidates`chứa mọi chỉ mục mảng vì chưa có vị trí nào bị loại trừ. 

Đối với mỗi bit, các giá trị có thể được phân vùng trước khi đặt câu hỏi. Điều này cung cấp dân số dự kiến ​​​​chính xác của từng nhóm bit. Sau đó, các vị trí mảng được truy vấn lần lượt và các câu trả lời tạo thành các nhóm được quan sát tương ứng. 

Sự so sánh`len(candidate_zero) < len(possible_zero)`là quyết định trọng tâm. Không thể có hai giá trị bị thiếu, vì vậy nếu nhóm 0 nhỏ hơn mong đợi thì thành viên còn thiếu của nó phải là câu trả lời. Nếu không nhỏ hơn thì chắc chắn nhóm đó đang thiếu một thành viên. 

Bit trả lời chỉ được đặt khi một nhóm được chọn. Không cần phải xây dựng câu trả lời một cách rõ ràng từ`possible_values`, mặc dù sau lần lặp cuối cùng, danh sách đó chỉ chứa đúng một số.`idx + 1`là cần thiết vì Python lưu trữ các chỉ số mảng từ 0 trong khi bộ tương tác đánh số vị trí từ 1. Bản thân bit này không được dịch chuyển hoặc điều chỉnh vì câu lệnh đánh số bit bắt đầu từ 0. 

Vòng lặp sử dụng`n.bit_length()`còn hơn là`int(log2(n))`. Với n=4,`n.bit_length()`là 3, do đó các bit 0,1,2 được xử lý và giá trị 4=100 2 ​ được xử lý chính xác. Số nguyên Python không bị tràn nên không cần loại số nguyên đặc biệt. 

các`flush=True`trên mọi truy vấn là bắt buộc trong một vấn đề tương tác. Nếu không có nó, Python có thể giữ truy vấn trong bộ đệm đầu ra trong khi chương trình chờ phản hồi của người tương tác, gây ra bế tắc. Tuyên bố yêu cầu rõ ràng mọi thông báo tương tác phải được xóa. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là bản ghi tương tác chứ không phải là đầu vào mảng thông thường. Các phản hồi của nó nhất quán, chẳng hạn như với mảng ẩn`[0, 3, 1]`, giá trị còn thiếu của nó là`2`. Mẫu hỏi ba câu hỏi và nhận được câu trả lời`0`,`1`, Và`0`, cuối cùng sản xuất`2`. 

| Chút | Các giá trị có thể có trước | Truy vấn vị trí ứng viên | Nhóm phản hồi | Lựa chọn bit | Các giá trị có thể có sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0, 1, 2, 3 | 1, 2, 3 | không: 1, 3; một: 2 | 0 | 0, 2 | 
| 1 | 0, 2 | 1, 3 | không: 1, 3; một: không có | 1 | 2 | 

Bảng này minh họa cơ chế chung, mặc dù bản ghi mẫu chính xác đặt các câu hỏi theo một thứ tự khác. Tại bit 0, giá trị`0`Và`2`thuộc nhóm 0, trong khi các giá trị`1`Và`3`thuộc về một nhóm. Mảng được quan sát chỉ chứa một thành viên của một nhóm, do đó giá trị còn thiếu có bit 0. Tại bit 1, trong số các khả năng còn lại`0`Và`2`, giá trị còn thiếu phải có bit 1, để lại`2`. 

Đối với ví dụ thứ hai, lấy n=4 và mảng ẩn`[0, 1, 2, 3]`. Giá trị còn thiếu là`4`. 

| Chút | Các giá trị có thể có trước | Kích thước nhóm bằng không | Một quy mô nhóm | Quan sát số 0 | Quan sát một | Lựa chọn bit | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0, 1, 2, 3, 4 | 3 | 2 | 2 | 2 | 0 |
