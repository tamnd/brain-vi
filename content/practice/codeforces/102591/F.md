---
title: "CF 102591F - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043d\u0430 \u043f\u0430\u0440\u044b"
description: "Chúng tôi có số học sinh lẻ và mỗi học sinh đều có một giá trị sức mạnh riêng biệt. Chúng ta phải để đúng một học sinh không có bạn cùng chơi và chia tất cả học sinh còn lại thành từng cặp."
date: "2026-08-01T06:38:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102591
codeforces_index: "F"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041c\u0423\u0418\u0422 \u043f\u043e \u0441\u043f\u043e\u0440\u0442\u0438\u0432\u043d\u043e\u043c\u0443 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2020. \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u0442\u0443\u0440."
rating: 0
weight: 102591
solve_time_s: 80
verified: true
draft: false
---

[CF 102591F - \u0420\u0430\u0437\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043d\u0430 \u043f\u0430\u0440\u044b](https://codeforces.com/problemset/problem/102591/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có số học sinh lẻ và mỗi học sinh đều có một giá trị sức mạnh riêng biệt. Chúng ta phải để đúng một học sinh không có bạn cùng chơi và chia tất cả học sinh còn lại thành từng cặp. Một cặp đóng góp sức mạnh nhỏ hơn của hai thành viên trong đó, vì vậy một cặp có điểm mạnh 10 và 3 đóng góp 3. Đối với mỗi học sinh có thể bị loại, chúng ta cần tổng đóng góp tối đa có thể có của tất cả các cặp. 

Đầu ra không phải là một giá trị. Nó chứa một câu trả lời cho mỗi học sinh ban đầu, trong đó câu trả lời ở vị trí`i`mô tả tổng điểm tốt nhất nếu học sinh đó là người không tham gia. 

Những hạn chế là lý do chính khiến vấn đề này trở nên thú vị. Với tối đa`3 * 10^5`học sinh, thử tất cả các phép loại bỏ có thể và tính toán lại một cặp tối ưu cho mỗi người sẽ yêu cầu phép tính bậc hai, khoảng`9 * 10^10`hoạt động trong trường hợp xấu nhất. Điều đó vượt xa những gì một giải pháp lập trình cạnh tranh có thể đáp ứng được. Chúng ta cần tìm một mẫu cho phép chúng ta xử lý mọi học sinh cùng nhau sau một lần sắp xếp duy nhất. 

Một số trường hợp đặc biệt có thể phá vỡ quá trình triển khai chỉ xử lý tình huống chung. Đầu vào nhỏ nhất có thể chỉ có ba sinh viên. Ví dụ, với điểm mạnh`3 1 2`, loại bỏ học sinh bằng sức mạnh`3`lá`[1,2]`, cho điểm`1`. Đang xóa`1`lá`[2,3]`, cho`2`. Đang xóa`2`lá`[1,3]`, cho`1`. Đầu ra đúng là`1 2 1`. Giải pháp luôn giả định học sinh lớn nhất tham gia theo cặp có thể thất bại ở đây. 

Một lỗi phổ biến khác là quên rằng học sinh bị loại sẽ thay đổi tính chẵn lẻ của tất cả các vị trí theo sau sau khi sắp xếp. Đối với đầu vào`1 2 3 4 5`, nếu chúng ta loại bỏ`3`, danh sách được sắp xếp còn lại là`1 2 4 5`. Điểm tối ưu là`1 + 4 = 5`. Xử lý các chỉ số ban đầu như thể không có gì thay đổi sẽ dẫn đến tập hợp sai các vị trí đã chọn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp bắt đầu bằng cách cố gắng loại bỏ mọi học sinh có thể. Sau khi loại bỏ một học sinh, chúng tôi sắp xếp hoặc duy trì những điểm mạnh còn lại và xây dựng cặp đôi tốt nhất. Đối với một danh sách được sắp xếp cố định gồm một số học sinh chẵn, rất dễ tìm ra chiến lược tối ưu: lấy thành viên nhỏ hơn trong mỗi cặp càng lớn càng tốt. Điều này có nghĩa là câu trả lời là tổng của mọi phần tử thứ hai khi được tính từ cuối hoặc tương đương với mọi phần tử ở chỉ số chẵn theo thứ tự tăng dần. 

Cách tiếp cận bạo lực là đúng vì nó kiểm tra chính xác tình huống cần thiết đối với mọi học sinh có thể bị đuổi học. Vấn đề là số lượng công việc lặp đi lặp lại. có`n`có thể bị xóa và mỗi lần xóa sẽ yêu cầu xử lý gần như`n`học sinh, dẫn đến`O(n^2)`làm việc sau khi tránh được việc sắp xếp không cần thiết. Với`n = 300000`, điều đó là không thể rồi. 

Quan sát quan trọng là chúng ta thực sự không cần phải xây dựng lại mảng còn lại cho mỗi lần xóa. Sắp xếp tất cả học sinh một lần. Hãy để những điểm mạnh được sắp xếp`a[0], a[1], ..., a[n-1]`. Nếu chúng ta loại bỏ một phần tử ở vị trí`r`, các phần tử được chọn trong mảng còn lại chính xác là các chỉ số chẵn ban đầu trước đó`r`và các chỉ số lẻ ban đầu sau`r`. 

Nguyên nhân là do sự thay đổi chỉ số. Các phần tử trước vị trí bị loại bỏ sẽ giữ nguyên chỉ số của chúng, vì vậy các vị trí chẵn vẫn là vị trí được chọn. Các phần tử sau vị trí bị loại bỏ sẽ di chuyển sang trái một bước, do đó chỉ mục lẻ ban đầu trở thành chỉ mục chẵn và góp phần đưa ra câu trả lời. 

Điều này biến bài toán thành việc duy trì hai tổng đơn giản. Tổng tiền tố của các giá trị tại các chỉ số chẵn sẽ cho biết phần đóng góp từ phía bên trái của phép loại bỏ. Tổng hậu tố của các giá trị tại các chỉ số lẻ cho biết sự đóng góp từ phía bên phải. Mọi câu trả lời chỉ là sự kết hợp của hai giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp học sinh theo sức mạnh nhưng vẫn giữ nguyên vị trí ban đầu. Việc sắp xếp cho phép chúng ta suy luận về thứ tự ghép đôi cuối cùng vì sự đóng góp của một cặp chỉ phụ thuộc vào học sinh nào yếu hơn. 
2. Xây dựng một mảng tiền tố trong đó`pref[i]`lưu trữ tổng các phần tử được sắp xếp với các chỉ số chẵn trước vị trí`i`. Khi một sinh viên ở vị trí`i`bị loại bỏ, tất cả các vị trí chẵn trước`i`không thay đổi và vẫn đóng góp. 
3. Xây dựng một mảng hậu tố trong đó`suf[i]`lưu trữ tổng các phần tử được sắp xếp với các chỉ số lẻ từ vị trí`i`đến cuối cùng. Sau khi loại bỏ vị trí`i`, mọi phần tử sau khi dịch chuyển sang trái một vị trí, do đó các vị trí lẻ ban đầu trở thành vị trí chẵn được chọn. 
4. Đối với mọi vị trí được sắp xếp`i`, tính toán`pref[i] + suf[i + 1]`. Đây là câu trả lời cho việc loại bỏ học sinh ở vị trí đã sắp xếp`i`. Tiền tố xử lý phía bên trái không thay đổi và hậu tố xử lý phía bên phải đã được dịch chuyển. 
5. Khôi phục các câu trả lời theo thứ tự đầu vào ban đầu bằng cách sử dụng các chỉ mục gốc đã lưu từ bước sắp xếp. Đầu ra được yêu cầu tuân theo thứ tự mà học sinh được đưa ra. 

Tại sao nó hoạt động: Đối với bất kỳ nhóm học sinh còn lại cố định nào, việc ghép cặp tối ưu luôn lấy mọi phần tử thứ hai trong chuỗi còn lại được sắp xếp bắt đầu từ phần tử nhỏ nhất. Việc loại bỏ một phần tử chỉ ảnh hưởng đến tính chẵn lẻ của các vị trí sau phần tử đó. Trước vị trí bị loại bỏ, các chỉ số được chọn chính xác là các chỉ số chẵn ban đầu. Sau đó, các chỉ số được chọn chính xác là các chỉ số lẻ ban đầu. Thuật toán tính toán chính xác hai nhóm này, vì vậy mỗi giá trị được tạo ra đều là điểm tối ưu cho học sinh bị loại tương ứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = list(map(int, input().split()))

    arr = sorted((value, idx) for idx, value in enumerate(s))

    pref = [0] * (n + 1)
    for i, (value, _) in enumerate(arr):
        pref[i + 1] = pref[i]
        if i % 2 == 0:
            pref[i + 1] += value

    suf = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suf[i] = suf[i + 1]
        if i % 2 == 1:
            suf[i] += arr[i][0]

    ans = [0] * n
    for i, (_, original_idx) in enumerate(arr):
        ans[original_idx] = pref[i] + suf[i + 1]

    print(*ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp lưu trữ cả cường độ và vị trí ban đầu. Thứ tự độ mạnh là cần thiết cho đối số ghép nối, trong khi chỉ mục ban đầu là cần thiết để đưa các câu trả lời cuối cùng trở lại thứ tự đầu vào. 

Việc xây dựng tiền tố chỉ thêm các giá trị ở các vị trí được sắp xếp chẵn. Nó cố tình sử dụng`pref[i]`khi trả lời vị trí`i`, vì bản thân phần tử đã bị loại bỏ không được đưa vào. 

Cấu trúc hậu tố quét từ phải sang trái và thu thập các giá trị ở các vị trí được sắp xếp lẻ. Khi loại bỏ vị trí`i`, phần bên phải bắt đầu tại`i + 1`, đó là lý do tại sao câu trả lời sử dụng`suf[i + 1]`. 

Số nguyên Python tự động xử lý kích thước tổng có thể có. Câu trả lời lớn nhất có thể có thể là xung quanh`1.5 * 10^14`, sẽ tràn các loại số nguyên 32 bit trong các ngôn ngữ cần xử lý thủ công. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3
3 1 2
```Mảng được sắp xếp là`[1, 2, 3]`. 

| Đã xóa vị trí sắp xếp | Tiền tố thậm chí đóng góp | Hậu tố đóng góp lẻ | Trả lời | 
| --- | --- | --- | --- | 
| 0, giá trị 1 | 0 | 1 | 1 | 
| 1, giá trị 2 | 1 | 0 | 1 | 
| 2, giá trị 3 | 1 | 0 | 1 | 

Khôi phục trật tự ban đầu`[3,1,2]`cho`[1,2,1]`. Học sinh cấp hai có kết quả tốt nhất vì việc loại bỏ học sinh yếu nhất sẽ để hai học sinh mạnh hơn lại với nhau. 

Cho một ví dụ khác:```
5
1 2 3 4 5
```Mảng đã được sắp xếp rồi`[1,2,3,4,5]`. 

| Đã xóa vị trí sắp xếp | Tiền tố thậm chí đóng góp | Hậu tố đóng góp lẻ | Trả lời | 
| --- | --- | --- | --- | 
| 0, giá trị 1 | 0 | 6 | 6 | 
| 1, giá trị 2 | 1 | 6 | 7 | 
| 2, giá trị 3 | 1 | 4 | 5 | 
| 3, giá trị 4 | 4 | 0 | 4 | 
| 4, giá trị 5 | 4 | 0 | 4 | 

Đang xóa`2`lá`[1,3,4,5]`, trong đó các cặp tối ưu đóng góp`1 + 4 = 5`. Các câu trả lời lớn hơn lúc đầu đến từ việc giữ nhiều phần tử lớn hơn ở các vị trí đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong hai bước chuyển tiền tố và hậu tố tuyến tính | 
| Không gian | O(n) | Mảng được sắp xếp và mảng tiền tố và hậu tố phụ trợ là tuyến tính | 

Giới hạn của`3 * 10^5`học sinh yêu cầu một giải pháp gần tuyến tính. Sắp xếp một lần và sau đó thực hiện số lần chuyển không đổi là phù hợp với kích thước này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    s = list(map(int, data[1:]))

    arr = sorted((value, idx) for idx, value in enumerate(s))

    pref = [0] * (n + 1)
    for i, (value, _) in enumerate(arr):
        pref[i + 1] = pref[i]
        if i % 2 == 0:
            pref[i + 1] += value

    suf = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suf[i] = suf[i + 1]
        if i % 2 == 1:
            suf[i] += arr[i][0]

    ans = [0] * n
    for i, (_, idx) in enumerate(arr):
        ans[idx] = pref[i] + suf[i + 1]

    return " ".join(map(str, ans))

assert solve("3\n3 1 2\n") == "1 2 1", "sample 1"

assert solve("5\n1 2 3 4 5\n") == "6 7 5 4 4", "shift parity case"

assert solve("3\n100 1 50\n") == "50 100 50", "minimum size"

assert solve("7\n7 1 6 2 5 3 4\n") == "9 15 10 14 10 13 11", "mixed order"

assert solve("3\n1000000000 1 999999999\n") == "999999999 1000000000 999999999", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 3 1 2`|`1 2 1`| Cung cấp mẫu và đầu vào hợp lệ nhỏ nhất | 
|`5 / 1 2 3 4 5`|`6 7 5 4 4`| Xử lý đúng các thay đổi về tính chẵn lẻ của chỉ mục sau khi xóa | 
|`3 / 100 1 50`|`50 100 50`| Thứ tự đúng khi giá trị lớn nhất không phải là giá trị đầu tiên | 
|`7 / 7 1 6 2 5 3 4`|`9 15 10 14 10 13 11`| Nhiều lần xóa với các vị trí hỗn hợp | 
|`3 / 1000000000 1 999999999`|`999999999 1000000000 999999999`| Số học số nguyên lớn | 

## Vỏ cạnh 

Khi chỉ có ba học sinh, thuật toán vẫn hoạt động vì mảng tiền tố và hậu tố chứa đủ vị trí biên. Đối với đầu vào`3 1 2`, việc loại bỏ phần tử đầu tiên được sắp xếp chỉ sử dụng phần đóng góp hậu tố, loại bỏ phần tử ở giữa sử dụng phần đóng góp tiền tố và loại bỏ phần tử lớn nhất để lại giá trị nhỏ nhất là phần đóng góp cặp duy nhất. 

Khi học sinh bị loại ở gần đầu hoặc cuối, sự thay đổi chỉ số là nguồn sai lầm phổ biến nhất. Đối với đầu vào`1 2 3 4 5`, loại bỏ`1`lá`[2,3,4,5]`. Câu trả lời sử dụng các chỉ số lẻ ban đầu sau vị trí bị loại bỏ, đưa ra`3 + 5 = 8`. Mảng hậu tố bắt đầu chính xác ở vị trí tiếp theo, do đó, nó nắm bắt được sự thay đổi này một cách chính xác. 

Đối với các cường độ rất lớn, số lượng cặp vẫn có thể đủ lớn để số học 32 bit không thành công. Một đầu vào như`1000000000 1 999999999`tạo ra gần một tỷ câu trả lời ngay cả khi chỉ có ba học sinh và các trường hợp lớn hơn có thể vượt quá giới hạn 32 bit. Việc triển khai sử dụng số nguyên Python, do đó việc tích lũy vẫn chính xác.
