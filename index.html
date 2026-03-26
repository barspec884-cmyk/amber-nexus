// Vercel Serverless Function — Amber Nexus 統合版
// 全ステップのデータを1つのドキュメントで管理

const { initializeApp, getApps, cert } = require('firebase-admin/app');
const { getFirestore, FieldValue } = require('firebase-admin/firestore');

// Firebase Admin 初期化
if (!getApps().length) {
  const serviceAccount = JSON.parse(process.env.FIREBASE_SERVICE_ACCOUNT);
  initializeApp({
    credential: cert(serviceAccount),
    projectId: 'amber--nexus'
  });
}
const db = getFirestore();

module.exports = async function handler(req, res) {
  // CORS設定
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, DELETE, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  if (req.method === 'OPTIONS') {
    return res.status(200).end();
  }

  try {
    // ============================================
    // GET: データ読み込み
    // ============================================
    if (req.method === 'GET') {
      const action = req.query.action || 'get';

      // --- 全ドキュメント一覧（Dashboard用）---
      if (action === 'list') {
        const limit = parseInt(req.query.limit) || 100;
        const statusFilter = req.query.status || null;

        let query = db.collection('tasting_sessions')
          .orderBy('updatedAt', 'desc')
          .limit(limit);

        const snapshot = await query.get();
        const docs = [];
        snapshot.forEach(doc => {
          const d = doc.data();
          if (statusFilter && d.status !== statusFilter) return;
          docs.push({
            id: doc.id,
            ...d,
            updatedAt: d.updatedAt?.toDate?.()?.toISOString() || null,
            createdAt: d.createdAt?.toDate?.()?.toISOString() || null,
          });
        });

        return res.status(200).json({ count: docs.length, docs });
      }

      // --- 単一ドキュメント取得 ---
      const docId = req.query.doc;
      if (!docId) {
        return res.status(400).json({ error: 'doc parameter is required' });
      }
      const snap = await db.collection('tasting_sessions').doc(docId).get();
      if (!snap.exists) {
        return res.status(200).json({ exists: false, data: null });
      }
      const data = snap.data();
      if (data.updatedAt?.toDate) data.updatedAt = data.updatedAt.toDate().toISOString();
      if (data.createdAt?.toDate) data.createdAt = data.createdAt.toDate().toISOString();
      return res.status(200).json({ exists: true, id: snap.id, data });
    }

    // ============================================
    // POST: データ保存
    // ============================================
    if (req.method === 'POST') {
      const { docId, data, merge, action } = req.body;

      // --- 新規セッション作成 ---
      if (action === 'create') {
        const saveData = {
          status: 'draft',
          currentStep: 1,
          ...data,
          createdAt: FieldValue.serverTimestamp(),
          updatedAt: FieldValue.serverTimestamp()
        };
        const docRef = await db.collection('tasting_sessions').add(saveData);
        return res.status(200).json({ success: true, id: docRef.id });
      }

      // --- 既存ドキュメント更新 ---
      if (!docId || !data) {
        return res.status(400).json({ error: 'docId and data are required' });
      }

      // 許可するフィールド（全ステップ対応）
      const allowed = [
        'status', 'currentStep',
        'step1', 'step2', 'step3', 'step4', 'step5',
        // 旧互換（Step2単独保存用）
        'tags', 'selectedColor', 'selectedGlass', 'sliders',
      ];

      const filtered = {};
      for (const key of allowed) {
        if (data[key] !== undefined) {
          filtered[key] = data[key];
        }
      }
      filtered.updatedAt = FieldValue.serverTimestamp();

      await db.collection('tasting_sessions').doc(docId).set(filtered, { merge: true });
      return res.status(200).json({ success: true });
    }

    // ============================================
    // DELETE: ドキュメント削除
    // ============================================
    if (req.method === 'DELETE') {
      const docId = req.query.doc;
      if (!docId) {
        return res.status(400).json({ error: 'doc parameter is required' });
      }
      await db.collection('tasting_sessions').doc(docId).delete();
      return res.status(200).json({ success: true });
    }

    return res.status(405).json({ error: 'Method not allowed' });
  } catch (e) {
    console.error('Firestore API error:', e);
    return res.status(500).json({ error: e.message });
  }
};
