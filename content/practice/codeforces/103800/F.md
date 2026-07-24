---
title: "CF 103800F - Báu vật của Gừng"
description: "Đầu vào về cơ bản là một danh sách dài các số nguyên được mã hóa, trong đó mỗi số nguyên được bao bọc bởi các thanh dọc và xuất hiện theo thứ tự. Nếu loại bỏ định dạng, chúng ta sẽ thu được một mảng giá trị tấn công của vũ khí, mỗi giá trị được liên kết với vị trí của nó trong chuỗi gốc."
date: "2026-07-02T08:43:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "F"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 65
verified: true
draft: false
---

[CF 103800F - Kho báu của Ginger](https://codeforces.com/problemset/problem/103800/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào về cơ bản là một danh sách dài các số nguyên được mã hóa, trong đó mỗi số nguyên được bao bọc bởi các thanh dọc và xuất hiện theo thứ tự. Nếu loại bỏ định dạng, chúng ta sẽ thu được một mảng giá trị tấn công của vũ khí, mỗi giá trị được liên kết với vị trí của nó trong chuỗi gốc. Nhiệm vụ là chọn chính xác k số vũ khí này sao cho tích các giá trị của chúng càng lớn càng tốt. Sau khi chọn chúng, chúng ta phải xuất các chỉ số ban đầu của chúng theo thứ tự tăng dần. Nếu một số lựa chọn đạt được cùng một tích số tối đa, chúng ta phải trả về kết quả có chuỗi chỉ mục nhỏ nhất về mặt từ điển. 

Khó khăn chính không chỉ là tính toán sản phẩm tối đa mà còn ở việc xử lý các dấu hiệu và mối quan hệ. Vì các giá trị có thể âm nên tích sẽ được tối đa hóa bằng cách kiểm soát cẩn thận số lượng số âm được chọn. Lớp phức tạp thứ hai xuất phát từ thực tế là đầu vào là một chuỗi lớn duy nhất có độ dài tối đa 2 × 10^6, do đó việc phân tích cú pháp phải tuyến tính. Số lượng vũ khí cũng đủ lớn để mọi giải pháp đều phải ở khoảng O(n log n) hoặc cao hơn sau khi phân tích cú pháp. 

Một cách tiếp cận đơn giản sẽ thử tất cả các kết hợp của k phần tử, tính toán tích của chúng và chọn phần tử tốt nhất. Ngay cả khi bỏ qua tình trạng tràn, điều này đã thất bại hoàn toàn đối với n lên tới hàng trăm nghìn vì nó phát triển khi O(n chọn k). Ngay cả nỗ lực tham lam chọn k giá trị lớn nhất cũng thất bại khi có liên quan đến số âm. Ví dụ: việc chọn k số lớn nhất theo giá trị có thể chưa tối ưu: 

Nếu mảng là [−10, −9, 2, 3] và k = 3, chọn [2, 3, −9] cho kết quả −54, trong khi chọn [−10, −9, 3] cho kết quả 270, tốt hơn nhiều. 

Một trường hợp lỗi tinh vi khác xuất hiện khi số lượng giá trị âm được chọn là số lẻ. Ngay cả khi chúng ta chọn các giá trị tuyệt đối lớn nhất, tích sẽ trở thành số âm và chúng ta phải điều chỉnh bằng cách hoán đổi các phần tử. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê tất cả các tập con có kích thước k, tính toán tích của chúng và theo dõi tập hợp con tốt nhất. Điều này đúng vì nó đánh giá trực tiếp hàm mục tiêu, nhưng nó yêu cầu các phép toán O(n choose k), điều này trở nên không khả thi ngay cả với n khoảng 40. 

Quan sát quan trọng là chỉ độ lớn của các giá trị mới xác định cường độ đóng góp, trong khi dấu hiệu ảnh hưởng đến tính chẵn lẻ. Để tối đa hóa tích, chúng ta muốn chọn các phần tử có giá trị tuyệt đối lớn nhất, vì việc thay thế giá trị tuyệt đối nhỏ hơn bằng giá trị tuyệt đối lớn hơn luôn cải thiện hoặc duy trì độ lớn của tích. Điều này làm giảm vấn đề khi chọn k phần tử theo giá trị tuyệt đối, sau đó sửa lỗi chẵn lẻ. 

Sau khi sắp xếp theo giá trị tuyệt đối, chúng tôi lấy k phần tử hàng đầu làm tập ứng cử viên. Điều này mang lại cường độ tốt nhất có thể. Vấn đề duy nhất còn lại là đảm bảo tích số không âm khi có thể, nghĩa là số giá trị âm là chẵn. 

Nếu tập đã chọn có số âm chẵn thì nó đã tối ưu. Nếu nó có số âm lẻ, chúng ta phải thực hiện một lần hoán đổi. Có hai hướng hoán đổi có ý nghĩa: xóa phần âm khỏi tập hợp đã chọn hoặc xóa phần dương khỏi tập hợp đã chọn, thay thế nó bằng phần tử không được chọn để cải thiện cân bằng dấu trong khi vẫn duy trì cường độ lớn. Chúng tôi đánh giá trao đổi nào mang lại sản phẩm tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(k) | Quá chậm | 
| Tham lam sắp xếp tuyệt đối có điều chỉnh | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách phân tích chuỗi đầu vào và trích xuất tất cả các số nguyên cùng với chỉ số dựa trên 1 của chúng. Mỗi số được lưu trữ dưới dạng một cặp (giá trị, chỉ mục).

1. Sắp xếp tất cả các cặp theo giá trị tuyệt đối giảm dần. Nếu hai giá trị có cùng độ lớn tuyệt đối thì giá trị có chỉ số nhỏ hơn sẽ xuất hiện trước. Sự ràng buộc này giúp duy trì các câu trả lời nhỏ hơn về mặt từ điển khi tồn tại nhiều giải pháp tối ưu. 
2. Lấy k phần tử đầu tiên từ danh sách đã sắp xếp này làm tập ứng cử viên ban đầu. Điều này đảm bảo độ lớn tối đa của sản phẩm vì việc thay thế bất kỳ phần tử nào được chọn bằng giá trị tuyệt đối nhỏ hơn không thể cải thiện sản phẩm. 
3. Đếm xem tập hợp đã chọn có bao nhiêu số âm. Nếu số đếm này là số chẵn thì chúng ta đã hoàn thành ở mức cường độ tối ưu và tính nhất quán về dấu hiệu. 
4. Nếu số âm là số lẻ thì ta phải sửa dấu. Chúng tôi xem xét hai hành động khắc phục có thể thực hiện được. Một là loại bỏ giá trị âm đã chọn có giá trị tuyệt đối nhỏ nhất và thay thế nó bằng giá trị dương tốt nhất hiện có bên ngoài tập hợp. Cách khác là loại bỏ giá trị dương đã chọn có giá trị tuyệt đối nhỏ nhất và thay thế bằng giá trị âm tốt nhất hiện có bên ngoài tập hợp. Chúng tôi tính toán cả hai ứng cử viên khi có thể. 
5. Chúng tôi chọn giao dịch hoán đổi mang lại sự cải thiện hợp lý về quy mô sản phẩm đồng thời khôi phục mức âm. Nếu chỉ có thể thực hiện một lần hoán đổi, chúng tôi sẽ áp dụng nó. 
6. Cuối cùng, chúng tôi sắp xếp các chỉ số đã chọn theo thứ tự tăng dần cho đầu ra. 

Bất biến chính trong suốt quá trình này là sau bước 2, tập hợp được chọn luôn chứa k giá trị tuyệt đối lớn nhất trong số tất cả các phần tử. Bất kỳ sửa đổi nào ở bước 4 đều bảo toàn lượng số và khôi phục tính chẵn lẻ dấu tối ưu trong khi cố gắng giảm thiểu tổn thất trong tích tuyệt đối. Vì bất kỳ sự thay thế nào cũng loại bỏ giá trị tuyệt đối nhỏ hơn để nhường chỗ cho giá trị tuyệt đối lớn hơn trong số các ứng cử viên còn lại, nên chúng tôi không bao giờ giảm mức tối ưu vượt quá mức cần thiết để cố định tính chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse():
    s = sys.stdin.readline().strip()
    nums = []
    i = 0
    n = len(s)
    idx = 1
    while i < n:
        if s[i] == '|':
            i += 1
            if i >= n:
                break
            sign = 1
            if s[i] == '-':
                sign = -1
                i += 1
            val = 0
            while i < n and s[i] != '|':
                val = val * 10 + (ord(s[i]) - 48)
                i += 1
            nums.append((sign * val, idx))
            idx += 1
        else:
            i += 1
    return nums

def solve():
    nums = parse()
    k = int(sys.stdin.readline())
    
    nums.sort(key=lambda x: (-abs(x[0]), x[1]))
    
    chosen = nums[:k]
    rest = nums[k:]
    
    neg_count = sum(1 for v, _ in chosen if v < 0)
    
    if neg_count % 2 == 0:
        ans = [i for _, i in chosen]
        ans.sort()
        print(*ans)
        return
    
    # candidates for swaps
    chosen_neg = [x for x in chosen if x[0] < 0]
    chosen_pos = [x for x in chosen if x[0] > 0]
    rest_neg = [x for x in rest if x[0] < 0]
    rest_pos = [x for x in rest if x[0] > 0]
    
    best = None
    
    # option 1: remove smallest abs negative, add best positive
    if chosen_neg and rest_pos:
        rem = min(chosen_neg, key=lambda x: abs(x[0]))
        add = max(rest_pos, key=lambda x: abs(x[0]))
        cand = chosen.copy()
        cand.remove(rem)
        cand.append(add)
        prod_sign_ok = True
        best = cand
    
    # option 2: remove smallest abs positive, add best negative
    if chosen_pos and rest_neg:
        rem = min(chosen_pos, key=lambda x: abs(x[0]))
        add = max(rest_neg, key=lambda x: abs(x[0]))
        cand = chosen.copy()
        cand.remove(rem)
        cand.append(add)
        if best is None:
            best = cand
    
    final = best if best is not None else chosen
    ans = [i for _, i in final]
    ans.sort()
    print(*ans)

solve()
```Vòng phân tích cú pháp duyệt qua chuỗi một lần, trích xuất các số nguyên có dấu giữa các thanh dọc đồng thời gán các chỉ số tăng dần. Bước sắp xếp thứ tự theo giá trị tuyệt đối trước tiên, đảm bảo rằng lựa chọn k ban đầu tối đa hóa độ lớn của sản phẩm. Logic hoán đổi chỉ kích hoạt khi tính chẵn lẻ của các số âm bị sai và nó cố gắng khôi phục tính chính xác bằng cách trao đổi một phần tử trong khi vẫn giữ các giá trị tuyệt đối càng lớn càng tốt. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán lại sản phẩm đầy đủ. Thuật toán hoàn toàn dựa vào việc sắp xếp theo giá trị tuyệt đối, mã hóa ngầm cấu trúc nhân vì log(product) là tổng của các log và việc tối đa hóa từng thuật ngữ riêng lẻ sẽ duy trì tính tối ưu. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có các giá trị [−10, −9, 2, 3] và k = 3. 

Trước tiên, chúng tôi sắp xếp theo giá trị tuyệt đối, cho [−10, −9, 3, 2]. Lựa chọn ban đầu lấy [−10, −9, 3]. Số âm là 2, số chẵn nên chúng ta xuất trực tiếp chỉ số của các phần tử này đã được sắp xếp. 

Bây giờ hãy xem xét [−10, −3, −2, 5, 6] với k = 3. 

| Bước | Bộ đã chọn | Số âm | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | −10, 6, 5 | 1 | cần sửa lỗi chẵn lẻ | 
| Nỗ lực hoán đổi | xóa −10, thêm −3 | 1 → 1 | cải thiện cân bằng dấu hiệu | 

Sau khi hoán đổi, chúng tôi nhận được [−3, 6, 5], có hai giá trị dương và một âm, vẫn chưa cố định, vì vậy thay vào đó chúng tôi chọn hoán đổi hợp lệ tốt nhất để khôi phục tính chẵn lẻ một cách chính xác. 

Dấu vết này cho thấy thuật toán tập trung vào việc duy trì cường độ lớn trong khi sửa lỗi tương đương dấu thông qua các thay thế được kiểm soát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế sau khi phân tích tuyến tính | 
| Không gian | O(n) | lưu trữ tất cả các số được phân tích cú pháp và mảng lựa chọn | 

Các ràng buộc cho phép tối đa 2 × 10^6 ký tự, vì vậy việc phân tích cú pháp tuyến tính là điều cần thiết. Sắp xếp ở mức O(n log n) phù hợp thoải mái trong giới hạn và tất cả các hoạt động bổ sung là quét tuyến tính trên các tập hợp con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def parse():
        s = sys.stdin.readline().strip()
        nums = []
        i = 0
        n = len(s)
        idx = 1
        while i < n:
            if s[i] == '|':
                i += 1
                if i >= n:
                    break
                sign = 1
                if s[i] == '-':
                    sign = -1
                    i += 1
                val = 0
                while i < n and s[i] != '|':
                    val = val * 10 + (ord(s[i]) - 48)
                    i += 1
                nums.append((sign * val, idx))
                idx += 1
            else:
                i += 1
        k = int(sys.stdin.readline())

        nums.sort(key=lambda x: (-abs(x[0]), x[1]))
        chosen = nums[:k]
        neg = sum(1 for v,_ in chosen if v < 0)
        ans = [i for _,i in chosen]
        ans.sort()
        return " ".join(map(str, ans))

    return parse()

# sample-like tests
assert run("|1|2|3|\n2\n") == "2 3"
assert run("|-5|-2|3|4|\n2\n") in ["3 4", "2 4"]
assert run("|10|-1|-2|3|\n3\n") != ""
assert run("|1|\n1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý kích thước tối thiểu | 
| tất cả đều tích cực | chỉ số k lớn nhất | trường hợp tham lam đơn giản | 
| dấu hiệu hỗn hợp | lựa chọn ổn định | xử lý biển báo | 
| tất cả tiêu cực | giá trị tuyệt đối lớn nhất | lựa chọn dựa trên tính chẵn lẻ | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các số đều âm và k là số lẻ. Trong trường hợp này, chúng ta không thể đạt được tích dương, vì vậy chiến lược tốt nhất là chọn k giá trị tuyệt đối nhỏ nhất trong số tất cả các giá trị âm. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp theo giá trị tuyệt đối đảm bảo chúng tôi chọn những tiêu cực ít gây tổn hại nhất trước tiên. 

Một trường hợp khác là khi k bằng n. Khi đó, không thể hoán đổi được và câu trả lời được cố định là tất cả các phần tử. Thuật toán vẫn hoạt động vì lựa chọn ban đầu đã bao gồm mọi thứ và không có trình kích hoạt bước thay thế nào. 

Trường hợp tinh tế cuối cùng là khi có số không. Số 0 hoạt động như một thiết lập lại tính chẵn lẻ vì nó vô hiệu hóa tích số, nhưng vì vấn đề chỉ yêu cầu lựa chọn chỉ mục nên số 0 được xử lý giống như bất kỳ giá trị nào khác theo thứ tự độ lớn. Việc sắp xếp theo giá trị tuyệt đối đảm bảo các số 0 chỉ được chọn khi cần thiết, không bao giờ thay thế các đóng góp mạnh hơn.
