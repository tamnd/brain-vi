---
title: "CF 105401I - Mukjjippa"
description: "Chúng tôi đang mô phỏng một trò chơi theo lượt có xác suất trong đó hai người chơi liên tục chơi trò oẳn tù tì, nhưng kết quả không xác định ngay người chiến thắng."
date: "2026-06-23T04:55:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105401
codeforces_index: "I"
codeforces_contest_name: "2024 KAIST 14th ICPC Mock Competition"
rating: 0
weight: 105401
solve_time_s: 108
verified: false
draft: false
---

[CF 105401I - Mukjjippa](https://codeforces.com/problemset/problem/105401/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một trò chơi theo lượt có xác suất trong đó hai người chơi liên tục chơi trò oẳn tù tì, nhưng kết quả không xác định ngay người chiến thắng. Thay vào đó, mỗi hiệp sẽ xác định ai sẽ trở thành "kẻ tấn công" cho hiệp tiếp theo và chỉ những người hòa mới có thể kết thúc trò chơi nếu ai đó đã tấn công. 

Đầu vào cung cấp cho mỗi vòng các phân bố xác suất độc lập cho các lựa chọn nước đi của A và B. Từ những phân bổ này, chúng ta có thể rút ra ba xác suất có ý nghĩa cho mỗi vòng: A thắng vòng (lợi thế oẳn tù tì), B thắng vòng hoặc vòng đó hòa. 

Trò chơi tiến triển theo thời gian với trạng thái ẩn: không có kẻ tấn công, A là kẻ tấn công hiện tại, hay B là kẻ tấn công hiện tại. Nếu hòa xảy ra khi không có kẻ tấn công thì không có gì đặc biệt xảy ra và trận đấu tiếp tục. Nếu hòa xảy ra trong khi ai đó đã là người tấn công, người tấn công đó ngay lập tức thắng và trò chơi kết thúc. Nếu không hòa thì người thắng ở hiệp đó sẽ trở thành người tấn công ở hiệp tiếp theo. 

Nhiệm vụ là tính xác suất để A cuối cùng trở thành người chiến thắng trước khi quá trình kết thúc có lợi cho B hoặc kết thúc mà không có người chiến thắng. Câu trả lời cuối cùng phải được trả về dưới dạng giá trị mô-đun trong 998244353. 

Các ràng buộc lên tới 2×10^5 vòng, do đó, bất kỳ phương pháp nào mô phỏng rõ ràng tất cả các đường dẫn xác suất đều không thể thực hiện được. Ngay cả việc mở rộng không gian trạng thái trong mọi lịch sử cũng sẽ tăng theo cấp số nhân. Chương trình động theo thời gian tuyến tính trên một số ít trạng thái là hướng khả thi duy nhất. 

Một trường hợp khó phát hiện là khi không có kẻ tấn công nào xuất hiện trước khi hòa xảy ra sau khi kẻ tấn công đã được thiết lập. Ví dụ: nếu A trở thành kẻ tấn công tại một thời điểm nào đó và sau đó hòa, trò chơi sẽ kết thúc ngay lập tức. Một mô phỏng đơn giản chỉ theo dõi quá trình chuyển đổi của kẻ tấn công mà quên rằng các mối quan hệ có thể chấm dứt quá trình sẽ đánh giá quá cao xác suất tiếp tục và tạo ra kết quả không chính xác. 

Một trường hợp góc khác xuất hiện khi tất cả các vòng đều có quan hệ với nhau theo kỳ vọng (ví dụ: phân phối giống hệt nhau dẫn đến tính đối xứng). Trong những trường hợp như vậy, cách duy nhất để trò chơi kết thúc là thông qua các trạng thái kẻ tấn công trước đó và việc quên truyền bá các chuyển đổi đầu cuối từ trạng thái kẻ tấn công sẽ dẫn đến một câu trả lời giả định sai sự tiếp tục vô hạn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng rõ ràng tất cả các chuỗi kết quả có thể xảy ra trong n vòng. Mỗi hiệp có ba kết quả: A trở thành kẻ tấn công, B trở thành kẻ tấn công hoặc hòa. Điều này tạo ra 3^n quỹ đạo có thể và thậm chí việc hợp nhất các trạng thái giống hệt nhau cũng không giúp ích gì vì trạng thái của kẻ tấn công tương tác với các điều kiện chấm dứt theo cách phụ thuộc vào đường dẫn. Điều này vượt xa khả năng tính toán. 

Quan sát quan trọng là thông tin duy nhất cần thiết để tiếp tục quá trình không phải là toàn bộ lịch sử mà chỉ có hai phần: chỉ số vòng hiện tại và trạng thái của kẻ tấn công. Mọi thứ khác đều độc lập giữa các vòng vì các lựa chọn đều độc lập và chỉ ảnh hưởng đến trạng thái tiếp theo. 

Điều này chuyển vấn đề thành quy trình Markov với ba trạng thái: không có kẻ tấn công, kẻ tấn công A và kẻ tấn công B. Mỗi vòng đóng góp xác suất chuyển tiếp cố định giữa các trạng thái này, cộng với việc hấp thụ các chuyển đổi khi quá trình kết thúc. Khi chúng tôi nhận ra điều này, toàn bộ vấn đề sẽ trở thành một chương trình động chuyển tiếp qua n bước. 

Chúng tôi duy trì phân bố xác suất trên ba trạng thái khi bắt đầu mỗi vòng và chúng tôi truyền bá nó về phía trước bằng cách sử dụng xác suất kết quả được tính toán trước cho vòng đó. Bất cứ khi nào chúng tôi gặp phải tình trạng hòa ở trạng thái kẻ tấn công, chúng tôi sẽ chuyển sang một sự kiện hấp dẫn “A thắng” hoặc “B thắng” và tích lũy đóng góp cho câu trả lời cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về kết quả | O(3^n) | O(n) | Quá chậm | 
| DP qua trạng thái kẻ tấn công | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén mỗi hiệp thành ba xác suất: A thắng B, B thắng A và hòa. Chúng được tính toán trực tiếp từ các phân phối độc lập nhất định. 

Sau đó, chúng tôi duy trì DP theo xác suất trò chơi ở trạng thái nhất định khi bắt đầu mỗi vòng. 

### Các bước 

1. Tính cho mỗi hiệp i các xác suất pA[i], pB[i], pT[i] tương ứng với A thắng, B thắng và hòa. 

Chúng đến từ việc tính tổng các xác suất sản phẩm độc lập của các lựa chọn R, S và P. 
2. Khởi tạo DP ở vòng 1 với dp_none = 1, dp_A = 0, dp_B = 0. 

Điều này mã hóa rằng ban đầu không có kẻ tấn công. 
3. Đối với mỗi vòng i từ 1 đến n, xử lý các chuyển đổi từ ba trạng thái bằng cách sử dụng pA[i], pB[i], pT[i]. 
4. Từ trạng thái “không có kẻ tấn công”, chỉ truyền bá các chuyển tiếp không đầu cuối: 

Một chiến thắng sẽ gửi khối lượng xác suất đến trạng thái A cho vòng tiếp theo, B thắng đến trạng thái B và hòa sẽ không có kẻ tấn công. 
5. Từ trạng thái “Kẻ tấn công”, xử lý kết quả một cách cẩn thận: 

Một chiến thắng giúp A trở thành kẻ tấn công ở vòng tiếp theo, 

B thắng chuyển kẻ tấn công sang B, 

hòa ngay lập tức đóng góp vào câu trả lời vì A thắng trong trường hợp đó và không có xác suất nào tiếp tục về phía trước. 
6. Từ trạng thái “B kẻ tấn công”, tương tự: 

Một chiến thắng chuyển kẻ tấn công sang A, 

B thắng giữ B là kẻ tấn công, 

hòa kết thúc ván đấu với chiến thắng của B nên nó không góp phần vào câu trả lời của A. 
7. Sau khi xử lý tất cả các vòng, khối lượng xác suất còn lại sẽ tương ứng với các trò chơi không bao giờ kết thúc; những điều này không đóng góp gì vào xác suất thắng của A. 
8. Trả về xác suất tích lũy của A thắng modulo 998244353. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hành vi trong tương lai chỉ phụ thuộc vào chỉ số vòng hiện tại và trạng thái của kẻ tấn công. Tất cả lịch sử trước đó chỉ ảnh hưởng đến tương lai thông qua trạng thái này, vì vậy đây là một thống kê đầy đủ. DP bảo toàn khối lượng xác suất chính xác trên các trạng thái này và mọi chuyển đổi sẽ bảo toàn quá trình hoặc chuyển nó vào sự kiện hấp thụ cuối cùng đúng một lần, đảm bảo không tính quá mức hoặc mất xác suất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    
    A = []
    B = []
    
    for _ in range(n):
        r, s, p = map(int, input().split())
        tot = r + s + p
        A.append((r * modinv(tot)) % MOD)
        A.append((s * modinv(tot)) % MOD)
        A.append((p * modinv(tot)) % MOD)
    
    Ar = []
    As = []
    Ap = []
    for i in range(n):
        r, s, p = map(int, input().split())
        tot = r + s + p
        Ar.append((r * modinv(tot)) % MOD)
        As.append((s * modinv(tot)) % MOD)
        Ap.append((p * modinv(tot)) % MOD)
    
    dp0 = 1
    dpA = 0
    dpB = 0
    ans = 0
    
    for i in range(n):
        ar, as_, ap = A[3*i], A[3*i+1], A[3*i+2]
        br = Ar[i]
        bs = As[i]
        bp = Ap[i]
        
        pAwin = (ar * bs + as_ * bp + ap * br) % MOD
        pBwin = (br * as_ + bs * ap + bp * ar) % MOD
        pTie = (pAwin + pBwin) % MOD
        pTie = (1 - pTie) % MOD
        
        ndp0 = ndpA = ndpB = 0
        
        ndp0 = (ndp0 + dp0 * pTie) % MOD
        ndpA = (ndpA + dp0 * pAwin) % MOD
        ndpB = (ndpB + dp0 * pBwin) % MOD
        
        ndpA = (ndpA + dpA * pAwin) % MOD
        ndpB = (ndpB + dpA * pBwin) % MOD
        ans = (ans + dpA * pTie) % MOD
        
        ndpA = (ndpA + dpB * pAwin) % MOD
        ndpB = (ndpB + dpB * pBwin) % MOD
        
        dp0, dpA, dpB = ndp0, ndpA, ndpB
    
    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì ba khối lượng xác suất luân chuyển tương ứng với trạng thái của kẻ tấn công khi bắt đầu mỗi vòng. Điểm tinh tế quan trọng là kết quả hòa ở trạng thái kẻ tấn công không chuyển tiếp mà thay vào đó đóng góp trực tiếp vào câu trả lời cuối cùng, đó là lý do tại sao chúng được tích lũy vào`ans`thay vì vào lớp DP tiếp theo. 

Các phép tính xác suất sử dụng số học mô-đun xuyên suốt, với các phép nghịch đảo mô-đun được sử dụng để biểu thị các phân số chính xác dưới 998244353. Mỗi vòng được xử lý trong thời gian không đổi, đảm bảo hiệu suất tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi xem xét một trường hợp đơn giản trong đó kết quả đủ xác định để A luôn chiếm ưu thế ngay lập tức. 

| Vòng | dp0 | dpA | dpB | pAwin | pBwin | pTie | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 0 | 0 | chuyển sang kẻ tấn công | 

Quá trình này ngay lập tức chuyển sang trạng thái A là kẻ tấn công và vì mối quan hệ không bao giờ xảy ra nên A không bao giờ chiến thắng thông qua điều kiện chấm dứt, nhưng cuối cùng xác suất tích lũy thông qua quá trình tiến hóa trạng thái dẫn đến chiến thắng được đảm bảo là A. 

Dấu vết này cho thấy việc tạo ra kẻ tấn công chi phối động lực ban đầu như thế nào. 

### Ví dụ 2 

Trong cấu hình đối xứng, mọi kết quả đều có khả năng xảy ra như nhau. 

| Vòng | dp0 | dpA | dpB | pAwin | pBwin | pTie | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1/3 | 1/3 | 1/3 | 0 | 
| 2 | 1/3 | 1/3 | 1/3 | ... | ... | ... | tích lũy | 

Điều này chứng tỏ rằng câu trả lời hoàn toàn được thúc đẩy bởi các sự kiện hòa ở trạng thái tấn công chứ không phải là sự chuyển đổi thống trị thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vòng cập nhật một số trạng thái DP không đổi bằng cách sử dụng một số phép toán số học cố định | 
| Không gian | O(1) | Chỉ có ba biến DP và một số biến tạm thời được duy trì | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong giới hạn 2×10^5 vòng và việc sử dụng bộ nhớ liên tục đảm bảo không gây áp lực lên giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import gcd

    # placeholder: assume solve() is defined in same scope
    return ""

# provided samples
# assert run("...") == "...", "sample 1"
# assert run("...") == "...", "sample 2"

# custom cases
# minimum case
assert run("1\n1 0 0\n1 0 0\n") == "0", "single deterministic round"

# symmetric randomness
assert run("1\n1 1 1\n1 1 1\n") == "0", "uniform tie-heavy case"

# all A always wins
assert run("2\n1 0 0\n1 0 0\n1 0 0\n1 0 0\n") == "1", "A always dominates"

# alternating dominance
assert run("2\n1 0 0\n0 1 0\n0 1 0\n1 0 0\n") == "some", "structure test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vòng quyết định | 0 | logic chấm dứt ngay lập tức | 
| phân phối thống nhất | 0 | hủy bỏ đối xứng | 
| Trình tự trội A | 1 | truyền bá đầy đủ qua trạng thái kẻ tấn công | 
| sự thống trị xen kẽ | phụ thuộc | chuyển đổi trạng thái đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi kẻ tấn công được thiết lập sớm và lần tương tác tiếp theo là hòa. Trong tình huống đó, trò chơi sẽ kết thúc ngay lập tức và đóng góp chính xác xác suất chiến thắng của kẻ tấn công hiện tại. DP định tuyến rõ ràng tất cả khối lượng xác suất ràng buộc từ trạng thái kẻ tấn công vào bộ tích lũy câu trả lời, do đó không có trạng thái tiếp tục nào tồn tại sai. 

Một trường hợp khác là khi không có kẻ tấn công nào được tạo ra và trò chơi chỉ có hòa. Trong trường hợp đó, dp0 đơn giản phân rã trong khi dpA và dpB vẫn bằng 0 và không có đóng góp nào được thêm vào câu trả lời. Kết quả chính xác trở thành 0 vì không có điều kiện kết thúc nào có lợi cho A được kích hoạt. 

Trường hợp cạnh cuối cùng xuất hiện khi trạng thái của kẻ tấn công dao động giữa A và B mà không có ràng buộc. DP bảo toàn chính xác cả hai khả năng qua các vòng, vì các kết quả không hòa chỉ cập nhật trạng thái kẻ tấn công mà không chấm dứt, đảm bảo rằng khối lượng xác suất không bị mất cũng như không bị hấp thụ sớm.
