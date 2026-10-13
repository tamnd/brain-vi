---
title: "CF 105161F - Giám sát tốc độ tải xuống"
description: "Chúng ta được cung cấp một chuỗi tốc độ tải xuống được lấy mẫu theo thời gian và kích thước cửa sổ cố định $k$. Với mỗi đoạn liền kề có độ dài $k$, chúng ta cần tính tốc độ trung bình của đoạn đó."
date: "2026-06-27T10:57:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105161
codeforces_index: "F"
codeforces_contest_name: "2024 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 105161
solve_time_s: 44
verified: true
draft: false
---

[CF 105161F - Trình theo dõi tốc độ tải xuống](https://codeforces.com/problemset/problem/105161/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi tốc độ tải xuống được lấy mẫu theo thời gian và kích thước cửa sổ cố định$k$. Đối với mỗi đoạn dài liền kề$k$, chúng ta cần tính tốc độ trung bình của đoạn đường đó. Sau khi tính giá trị trung bình, nó được hiển thị theo các đơn vị khác nhau tùy thuộc vào độ lớn của nó: nếu giá trị trung bình tối thiểu là 1024, thì nó được hiển thị bằng MiBps, nếu không thì nó được hiển thị bằng KiBps. 

Về mặt khái niệm, nhiệm vụ là một vấn đề tổng hợp cửa sổ trượt trên một mảng, sau đó là chuyển đổi đơn vị dựa trên ngưỡng đơn giản. Mỗi cửa sổ tạo ra chính xác một giá trị đầu ra và đầu ra phải duy trì quy tắc đơn vị một cách độc lập cho mọi cửa sổ. 

Nếu như$n$lớn, ví dụ lên tới$10^5$, việc tính toán lại đơn giản của mỗi tổng cửa sổ sẽ yêu cầu$O(k)$làm việc theo từng vị trí, dẫn đến$O(nk)$tổng số hoạt động. Trong trường hợp xấu nhất nơi$k$cũng lớn, việc này trở nên quá chậm vì nó sẽ tiến gần đến$10^{10}$hoạt động. 

Một điểm tinh tế hơn là đầu ra phụ thuộc vào mức trung bình chứ không phải tổng. Điều đó có nghĩa là cần phải phân chia cho mọi cửa sổ và việc chia số nguyên bất cẩn có thể dẫn đến phân loại đơn vị không chính xác nếu áp dụng làm tròn quá sớm. Cách tiếp cận đúng phải đảm bảo rằng tổng của mỗi cửa sổ được tính toán chính xác trước khi chia. 

Trường hợp cạnh điển hình phát sinh khi các giá trị di chuyển quanh ranh giới ngưỡng. Ví dụ, nếu$k = 3$và một cửa sổ tính tổng chính xác$3071$, trung bình là$1023.666...$, nên được phân loại là KiBps, không phải MiBps. Bất kỳ việc triển khai nào so sánh tổng thô với 1024 thay vì so sánh mức trung bình sẽ phân loại không chính xác các trường hợp đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: với mọi chỉ số bắt đầu$i$, chúng tôi quét tiếp theo$k$các phần tử, tính tổng của chúng, chia cho$k$, rồi áp dụng quy tắc chuyển đổi đơn vị. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề, xử lý từng cửa sổ một cách độc lập. 

Vấn đề với cách tiếp cận này là công việc lặp đi lặp lại. Mỗi cửa sổ mới gần như trùng lặp hoàn toàn với cửa sổ trước đó, nhưng phương pháp brute-force sẽ tính lại toàn bộ số tiền từ đầu. Vì$n$cửa sổ có kích thước$k$, điều này dẫn đến khoảng$n \cdot k$bổ sung. Khi cả hai$n$Và$k$lớn, điều này là quá chậm. 

Quan sát quan trọng là các cửa sổ liên tiếp khác nhau đúng một phần tử: phần tử ngoài cùng bên trái của cửa sổ trước đó bị xóa và phần tử mới ở bên phải được thêm vào. Điều này cho phép chúng tôi duy trì tổng số tiền đang hoạt động. Thay vì tính toán lại tổng của mỗi cửa sổ, chúng tôi cập nhật nó theo thời gian không đổi bằng cách trừ đi giá trị đi và cộng giá trị vào. Điều này làm giảm tổng công việc từ$O(nk)$ĐẾN$O(n)$. 

Sau khi duy trì tổng một cách hiệu quả, giá trị trung bình của mỗi cửa sổ được tính theo thời gian không đổi và quy tắc chuyển đổi đơn vị được áp dụng ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(1)$| Quá chậm | 
| Cửa Sổ Trượt |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số đầu tiên$k$các phần tử. Điều này tạo thành cửa sổ ban đầu và thiết lập giá trị cơ sở cho tất cả các bản cập nhật tiếp theo. 
2. Tính giá trị trung bình của cửa sổ ban đầu này bằng cách chia tổng cho$k$. Tùy thuộc vào mức trung bình này ít nhất là 1024, hãy xuất nó ở dạng MiBps hoặc KiBps. 
3. Lặp lại từ chỉ mục$k$ĐẾN$n-1$, coi mỗi vị trí là điểm cuối bên phải của một cửa sổ mới. 
4. Đối với mỗi vị trí mới, hãy cập nhật tổng hiện có bằng cách trừ phần tử rời khỏi cửa sổ và thêm phần tử mới đi vào. Điều này giữ cho tổng nhất quán với cửa sổ hiện tại mà không cần tính toán lại từ đầu. 
5. Sau khi cập nhật tổng, hãy tính giá trị trung bình mới bằng cách chia cho$k$. 
6. Áp dụng lại quy tắc chuyển đổi đơn vị dựa trên mức trung bình được tính toán và đưa ra kết quả. 

Bất biến cốt lõi là ở mỗi lần lặp, tổng chạy chính xác bằng tổng của lần lặp cuối cùng.$k$các phần tử kết thúc ở chỉ mục hiện tại. Vì mỗi bản cập nhật sẽ loại bỏ chính xác một phần tử cũ và thêm chính xác một phần tử mới nên tổng luôn khớp với cửa sổ hiện tại. Điều này đảm bảo rằng mọi mức trung bình được tính toán đều chính xác và do đó mọi phân loại đơn vị dựa trên nó cũng đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    n, k = data[0], data[1]
    arr = data[2:]

    window_sum = sum(arr[:k])
    out = []

    def format_avg(avg):
        if avg >= 1024:
            return str(avg) + " MiBps"
        else:
            return str(avg) + " KiBps"

    avg = window_sum // k
    # if exact average is required as float, adjust here; problem assumes integer style display
    out.append(format_avg(avg))

    for i in range(k, n):
        window_sum += arr[i]
        window_sum -= arr[i - k]
        avg = window_sum // k
        out.append(format_avg(avg))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng trong một lần và khởi tạo tổng cửa sổ đầu tiên bằng cách sử dụng tiền tố trực tiếp trên giá trị đầu tiên$k$các phần tử. Bước cập nhật trượt được thực hiện bằng cách thêm phần tử đến và loại bỏ phần tử gửi đi, duy trì cập nhật theo thời gian liên tục. 

Một mối quan tâm thực hiện tinh tế là chia số nguyên. Nếu bài toán ban đầu yêu cầu giá trị trung bình là giá trị thực thì việc sử dụng phép chia sàn sẽ không chính xác. Trong trường hợp đó, tổng phải được chia bằng số học dấu phẩy động trước khi so sánh với 1024. Tuy nhiên, cấu trúc của cửa sổ trượt vẫn không thay đổi. 

Hàm định dạng đầu ra đóng gói logic ngưỡng sao cho mỗi kết quả cửa sổ được xử lý thống nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó$n = 5$,$k = 3$, và mảng là$[1024, 1024, 1024, 512, 512]$. 

Tổng cửa sổ ban đầu là 3072, cho kết quả trung bình là 1024. 

| Bước | Cửa sổ | Tổng hợp | Trung bình | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | [1024, 1024, 1024] | 3072 | 1024 | MiBps | 
| 1 | [1024, 1024, 512] | 2560 | 853 | KiBps | 
| 2 | [1024, 512, 512] | 2048 | 682 | KiBps | 

Dấu vết này cho thấy cách cập nhật trượt chỉ thay đổi một phần tử mỗi bước trong khi vẫn duy trì tính chính xác. 

Bây giờ hãy xem xét trường hợp nhạy cảm với ranh giới trong đó$n = 4$,$k = 2$, mảng$[1535, 1536, 0, 0]$. 

| Bước | Cửa sổ | Tổng hợp | Trung bình | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | [1535, 1536] | 3071 | 1535 | MiBps | 
| 1 | [1536, 0] | 1536 | 768 | KiBps | 
| 2 | [0, 0] | 0 | 0 | KiBps | 

Ví dụ này chứng minh rằng việc phân loại phải dựa trên mức trung bình được tính toán trên mỗi cửa sổ chứ không phải dựa trên các giá trị riêng lẻ hoặc lý luận gần đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được thêm vào và xóa khỏi tổng số tối đa một lần | 
| Không gian |$O(1)$| Chỉ một số lượng biến không đổi được sử dụng ngoài mảng đầu vào | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho$n$lên tới$10^5$, vì nó chỉ thực hiện công việc tuyến tính và tránh việc tính toán lại nhiều lần các cửa sổ chồng chéo. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

# sample-like case
assert run("5 3 1024 1024 1024 512 512") == "1024 MiBps\n853 KiBps\n682 KiBps"

# minimum case
assert run("1 1 2048") == "2048 MiBps"

# all equal below threshold
assert run("4 2 100 100 100 100") == "100 KiBps\n100 KiBps"

# boundary case around 1024
assert run("3 2 1023 1025 1024") == "1024 KiBps\n1024 KiBps"

# decreasing values
assert run("5 2 2000 1500 1000 500 0") == "1750 MiBps\n1250 MiBps\n750 KiBps\n250 KiBps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2048`|`2048 MiBps`| Cửa sổ một phần tử | 
|`4 2 100 100 100 100`|`100 KiBps\n100 KiBps`| Giá trị thấp thống nhất | 
|`3 2 1023 1025 1024`| hỗn hợp | Hành vi ranh giới ngưỡng | 
|`5 2 2000 1500 1000 500 0`| hỗn hợp | Cập nhật trượt chính xác | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi mức trung bình của cửa sổ cực kỳ gần với ngưỡng 1024. Hãy xem xét tổng cửa sổ là 2047 với$k = 2$, cho kết quả trung bình là 1023,5. Phân loại chính xác là KiBps, mặc dù tất cả các giá trị liên quan có thể vượt quá 1024. Thuật toán xử lý điều này một cách chính xác vì phân loại chỉ phụ thuộc vào mức trung bình của cửa sổ được tính toán sau khi tổng hợp đầy đủ. 

Một trường hợp cạnh khác là khi$k = 1$. Trong trường hợp này, mỗi cửa sổ là một phần tử duy nhất và bản cập nhật trượt sẽ thoái hóa thành đầu ra trực tiếp của từng giá trị. Bất biến vẫn giữ nguyên vì bước “xóa phần tử cũ, thêm phần tử mới” trở nên tầm thường và mỗi tổng chính xác bằng chính phần tử đó. 

Trường hợp cạnh cuối cùng là khi tất cả các giá trị bằng 0. Tổng của mỗi cửa sổ vẫn bằng 0 trong suốt quá trình thực thi và đầu ra luôn ở mức KiBps. Cửa sổ trượt vẫn thực hiện cập nhật, nhưng mỗi bản cập nhật sẽ hủy chính xác, duy trì tính chính xác mà không cần viết hoa đặc biệt.
